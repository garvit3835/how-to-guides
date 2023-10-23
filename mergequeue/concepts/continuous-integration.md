# Continuous Integration

Aviator MergeQueue supports any CI system that integrates with [GitHub status checks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/collaborating-on-repositories-with-code-quality-features/about-status-checks).

## CI status requirements

In GitHub, you can require tests to be passing with [protected branches require status check](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches#require-status-checks-before-merging) before merging. Aviator MergeQueue uses the same data as GitHub status checks, but it allows you to set two different requirements on the status.

* Pre-queue status requirements
  * Same as the GitHub protected branch status requirement, this configuration is the status requirement before merging.
* Draft PR status requirements
  * When a pull-request enters a queue, MergeQueue creates a new temporary branch and a PR, which is called "draft PR". This draft PR contains the changes from the queued PR and the latest target branch (e.g. `main`) so that we can test against the post-merge state.

The first one sets requirements for pre-merge, and the second one sets requirements for post-merge. You can require the same sets of tests for both, or if needed, you can require different tests to be passing.

## See also

* [MQ Created Branches](optimizing-ci-execution.md) for additional details on the CI setup.
* [Pre-Queue Conditions](pre-queue-conditions.md) for other conditions you can set for the pre-queue time.
