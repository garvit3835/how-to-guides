# TestDeck

## What is a flaky test

A flaky test is a non-deterministic tests that pass and fail without any code changes. It causes builds to fail even though the developer hasn’t made any related changes. This requires manual reruns of full CI that results in unnecessary developer overhead and increases in CI compute cost.

## Overview

Aviator helps by identifying flaky tests and taking actions to reduce overhead. It has two main capabilities: reporting and auto-rerunning.

### Reporting

Aviator captures and analyzes test results from your preferred CI provider, processes each test case, and tracks performance and reliability of each test case. Here is an overview of capabilities that we offer.

* Identify flaky tests in the system reactively and proactively. These results are reported through our dashboard and using APIs and webhooks.
* Historical view of a particular test case - how often the test has failed (flaky or not), has the test become stable / unstable. Views by feature branches vs. base branches.
* Visibility into whether test stability is degrading or improving for base branches.
* Visibility into whether test run times are going increasing or decreasing (analyze P50, P90, etc. of test run times).
* Ability to proactively rerun the test suite at a particular cadence (nightly job) to identify flakes.

### **Auto-rerun unreliable tests**

By using Aviator’s plugins, you can automatically rerun unreliable or flaky tests and control the execution of the tests from a central platform. This significantly improves CI reliability and reduces costs by avoiding multiple reruns.

## Getting started

Aviator fetches the test results by using a custom plugin. Currently, we support CircleCI and Buildkite integrations. For both plugins, you need to specify two things: `AVIATOR_API_TOKEN` to associate the data with your account and the files associated with the test artifacts.

### CircleCI

To install Aviator on CircleCI, please use the Aviator orb defined [here](https://github.com/aviator-co/circleci-upload-orb).

**Step 1**: Configure the API token in the [environment variables section](https://circleci.com/docs/set-environment-variable/#set-an-environment-variable-in-a-project) as:

```
AVIATOR_API_TOKEN=av_live_xxxxxxx
```

**Step 2**: Add a step after your test execution step.

```yaml
description: >
  Add the `aviator-upload-orb/upload` command after
  getting test results in order to upload them to the Aviator server.
usage:
  version: 2.1
  orbs:
    aviator-upload-orb: aviator/aviator-upload-orb@0.0.3
  jobs:
    test:
      docker:
        - image: cimg/python:3.7
      steps:
        - checkout
        - run:
            name: Run tests and upload results
            command: |
              python -m pytest -vv --junitxml="test_results/output.xml"
        - store_artifacts:
            path: ./test_results/output.xml
            destination: output.xml
        - aviator-upload-orb/upload:
            assets: "test_results/*.xml"
  workflows:
    test-and-upload:
      jobs:
        - test
```

Once this is done, you should be able to verify that the artifacts are getting uploaded by looking at your CircleCI. You should see something like:

```bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 37559  100    60  100 37499     61  38680 --:--:-- --:--:-- --:--:-- 38720
{"success":true,"message":"Artifact uploaded successfully."}
CircleCI received exit code 0
```

### Buildkite

To install Aviator on Buildkite, please use the Buildkite plugin defined [here](https://github.com/buildkite-plugins/aviator-buildkite-plugin).

**Step 1**: Configure the API token in the [environment variables section](https://circleci.com/docs/set-environment-variable/#set-an-environment-variable-in-a-project) as:

```
AVIATOR_API_KEY=av_live_xxxxxxx
```

**Step 2**: Now you can either upload the artifacts from the same step as the test run or create a separate step:

**Option A: From build step**

To upload all files from an XML folder from a build step:

```yaml
steps:
  - label: "🔨 Test"
    command: "make test"
    plugins:
      - aviator#v1.0.0:
          files: "test/junit-*.xml"
```

**Option B: Using build artifacts**

You can also use build artifacts generated in a previous step:

```yaml
steps:
  # Run tests and upload 
  - label: "🔨 Test"command: "make test --junit=tests-N.xml"artifact_paths: "tests-*.xml"

  - wait

  - label: ":plane: Aviator"command: buildkite-agent artifact download tests-*.xml
    plugins:
      - aviator#v1.0.0:
          files: "tests-*.xml"
```

## TestDeck Dashboard

### Historical view

A golden branch is the protected base branch (typically referred to as `master` or `main`) that is assumed to be always passing. Any CI failures on this branch are deemed flaky. Occasional true failures in this branch can be manually flagged as such to keep data coherent.

Aviator reports the results of CI checks in a historical view, and provides stats around destabilization of builds. You can easily monitor the status of both the repo's golden branch and all other branches.

<figure><img src="../.gitbook/assets/Screen Shot 2023-03-23 at 2.59.33 PM.png" alt=""><figcaption><p>Test history in TestDeck</p></figcaption></figure>

### Build report

Other useful details include history of a specific test suite, as well as a particular build.

<figure><img src="../.gitbook/assets/Screen Shot 2023-03-23 at 2.59.04 PM.png" alt=""><figcaption></figcaption></figure>

## TestDeck API

{% swagger method="get" path="/flaky" baseUrl="https://api.aviator.co/api/v1/testdeck" summary="Get flaky tests in the last 30 days." %}
{% swagger-description %}
The flaky test API can be used to fetch the results of all flaky tests in the last 30 days. The results are paginated with maximum of 100 results returned in the response. The results are in the order of the most recent flaky, based on 

`first_occurrence`

.
{% endswagger-description %}

{% swagger-parameter in="body" name="org" type="String" required="true" %}
Organization name for the repo.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="repo" type="String" required="true" %}
Repository name.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="github_check_name" type="String" required="true" %}
Github check name.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="page" type="Integer" %}
Page number.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
  "repository": {
     "name": "repo_name",
     "org": "repo_org"
  },
  "github_check": {
    "name": "ci/circleci: unit-tests",
    "provider": "CircleCI"
  },
  "test_cases": [
    {
      "name": "test_invalid_security_code",
      "class_name": "core.payments.SecurityCodeTest",
      "first_occurrence": "2022-11-16T17:21:41.350499",
      "last_occurrence": "2022-11-18T11:13:23.12361",
      "details_url": "https://app.aviator.co/flaky/test_case/101063",
      "last_24_hours_count": 5
    },
    {
      "name": "test_valid_security_code",
      "class_name": "core.payments.ValidCodeTest",
      ...
    },
    ..
  ]
}
```
{% endswagger-response %}
{% endswagger %}

## TestDeck webhooks

Aviator already support a [webhook framework](https://docs.aviator.co/reference/webhooks) that lets you listen to various events. By using Aviator’s new capabilities to detect flaky tests, you can now subscribe to these flaky test webhooks to take actions based on the event payload.

### Event payload

Example JSON payload for webhook:

```json
{
  "action": "new_flaky_test",
  "repository": {
     "name": "repo_name",
     "org": "repo_org"
  },
  "test_suite": {
    "name": "ci/circleci: unit-tests",
    "provider": "CircleCI"
  },
  "test_case": {
    "name": "test_invalid_security_code",
    "class_name": "core.payments.SecurityCodeTest",
    "first_occurence": "2022-11-16T17:21:41.350499",
    "last_occurence": "2022-11-18T11:13:23.12361",
    "last_24_hours_count": 5
  },
  "commit_sha": "c576a61d26f72ad335660bdaefa4d63306c5752c"
}
```

Note: `commit_sha` is an optional field and will only be present when notifying about a `new_flaky_test`.

### Event actions

You can subscribe to these specific actions for webhooks.

* `new_flaky_test` - a new flaky test is found.
* `resolved_flaky_test` - when an existing flaky test has not been identified as flaky in the last 14 days.

## Slack Summary

You can also configure a daily summary to be reported via Slack for specific test suites. This requires the [Slack integration](https://docs.aviator.co/reference/slack-integration) to be active. The Slack summary can be enabled via the YAML configuration file:

```
testdeck:
  jobs:
    - name: buildkite/pytest
      daily_summary:
        publish_time: "14:00" # UTC time as HH:MM
```

The daily summary includes:

* Total number of flaky runs today
* Most flaky test case of the day
* Any new flaky tests introduced in the last 24 hours

## Nightly job

The nightly jobs are scheduled job runs that can be triggered using the [Pilot framework](https://docs.aviator.co/reference/pilot-automated-actions). These jobs can be used to identify flakes by running the same test on an existing green build. Nightly jobs are also configured using the YAML config file:

```yaml
scenarios:
- name: "nightly build"
  trigger:
    schedule:
      cron_utc: "0 12 * * *"  # Every day at 12pm UTC
  actions:
  - test_suite:
      rerun:
        suite_name: "buildkite/pytest"
        count: 5
        use_latest_green_commit: true
        branch: "master"
```

Note that the `cron_utc` config ignores the minute level precision. You can run it at most at an hourly cadence.

## Rerun tests

Currently, rerunning flaky tests is supported in the `pytest` framework - see plugin [here](https://github.com/aviator-co/pytest-aviator).

### Installation

**Step 1**: Install the plugin in your repository using pip or poetry:

```
pip install pytest-aviator
```

**Step 2**: Set your account's Aviator API token as an environment variable AVIATOR\_API\_TOKEN.

That’s it. Now when you run the tests using `pytest`, the plugin will automatically identify all flaky tests by querying the Aviator APIs and rerun depending on the configuration set in TestDeck.

## Add CI connections

Connect to your CI provider on the `Connections` page. We currently support CircleCI and Buildkite. You will need the provider API token in order for TestDeck to communicate with the provider. The instructions to generate API token can be count on the configuration page for the respective CI.

[CircleCI API Tokens](https://circleci.com/docs/managing-api-tokens)

[Buildkite API Tokens](https://buildkite.com/docs/apis/managing-api-tokens)

<figure><img src="../.gitbook/assets/Screen Shot 2022-09-01 at 4.42.01 PM.png" alt=""><figcaption><p>Integrate a CI provider.</p></figcaption></figure>



Want access to TestDeck? Please contact info@aviator.co.
