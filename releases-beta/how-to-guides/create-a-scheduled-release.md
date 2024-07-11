# Create a scheduled release

Aviator Releases currently works with GitHub Actions and Buildkite and the build and deploy workflows are triggered manually. In order to support scheduled releases to specific environments, the following APIs can be used.

The expectation is that you can run a scheduled job that calls [<mark style="color:blue;">Release APIs</mark>](../api-reference.md) to create releases and run the deploy workflows.

## APIs for Release Pipeline

A software release process would look like this:

1. A cron job is managed by the user, this job is triggered on a period cadence.
2. This cron job generates a new release candidate version name (e.g. `v2024.05.07-rc1`), and calls the API to create a new release candidate.
3. Aviator will verify the parameters and generate a new release object.
4. Once the release object is generated, a second API can create a deployment using this release version and the provided environment.

In the real world, you may add more steps in the pipeline, but they will have something similar to above steps in it.

***

To adopt Aviator Release, we need your pipeline to call our API twice.

1. To create a Release Candidate.
2. To start a Deployment.

To support this we provide the following two API endpoints:

* `POST /api/releases/$name/releases/$version`
  * POST body: `{ repo: "mycompany/myrepo", commit: "1a2b3c..." }`
  * This creates a new release with the specified version name. Note that the version name is the release candidate version and should end with `-rcX` where `X` is a number.
* `POST /api/releases/$name/environments/$envname/deployments`
  * POST body: `{ release_candidate_version: "v2024.05.07-rc1" }`
  * This triggers a new deploy workflow for the provided environment.

Read the [<mark style="color:blue;">full API reference guide</mark>](../api-reference.md) to learn more about these APIs.
