# FlakyBot

## Flaky tests

Flaky tests are non-deterministic tests that pass and fail without any code changes. It causes builds to fail even though the developer hasn’t made any related changes. This requires manual reruns of full CI that requires unnecessary developer overhead and increases in CI compute cost.

## Introducing FlakyBot

Aviator’s FlakyBot acts as a shock-absorber for flaky tests. It is an app that connects to your GitHub account and the CI provider (currently supports CircleCI, BuildKite, and Jenkins). It extracts the test artifacts for every CI run - these test artifacts are processed to identify each individual test status and reasons for failures. Based on the specified preconditions, Aviator’s FlakyBot will provide a custom report that can be used to pass / fail the builds.

FlakyBot also acts as a GitHub status check (CI check) on your PRs in addition to existing CI status checks. This FlakyBot CI check analyzes the CI test artifacts and produce a CI check report excluding flaky tests. This report can be used as a pass / fail requirement instead of the original CI status checks. The core FlakyBot engine uses a mix of heuristics and machine learning to flag each test result with root cause analysis.

The main goal of FlakyBot is to create automatic shock-absorbers for flaky tests while identifying true failures. As a result, developers do not have to worry about broken builds that are unrelated to his or her changes.

## Set up

### Connect a repo

Make sure you have the Aviator app connected to a repository. You can always add another via the `Repositories` page. Make sure you enable the repo on the `Flaky Tests` page.

<figure><img src="../.gitbook/assets/Screen Shot 2022-09-02 at 4.35.54 PM.png" alt=""><figcaption><p>Enable your repo.</p></figcaption></figure>

### Add connections

Connect to your CI provider on the `Connections` page. We currently support CircleCI and Buildkite. You will need the provider API token in order for FlakyBot to communicate with the provider. The instructions to generate API token can be count on the configuration page for the respective CI.

[CircleCI API Tokens](https://circleci.com/docs/managing-api-tokens)

[Buildkite API Tokens](https://buildkite.com/docs/apis/managing-api-tokens)

<figure><img src="../.gitbook/assets/Screen Shot 2022-09-01 at 4.42.01 PM.png" alt=""><figcaption><p>Integrate a CI provider.</p></figcaption></figure>

### Backfill

Once you have successfully connected a provider, you may need to wait a some time for FlakyBot to collect test data. We'll run some backfill jobs in the background to get your historical test data. Any current CI execution results should be captured in real-time.

## Overview

With Aviator's Flakybot, you can easily view the detailed history of your repo's flaky tests.

{% embed url="https://www.loom.com/share/e9d85809a7134440a50eeecc0049a8e5" %}

## Features

### **Historical Views**

A golden branch is the protected base branch (typically referred to as `master` or `main`) that is assumed to be always passing. Any failures of CI on this branch are deemed flaky. Occasional true failures in this branch can be manually flagged as such to keep data coherent.

FlakyBot reports the results of CI checks with a historical view, and provides stats around destabilization of builds. You can easily monitor the status of both the repo's golden branch and all other branches.

<figure><img src="../.gitbook/assets/Screen Shot 2022-09-02 at 5.02.00 PM.png" alt=""><figcaption><p>View test history.</p></figcaption></figure>

Other useful details include history of a specific test suite, as well as a particular build.

<figure><img src="../.gitbook/assets/Screen Shot 2022-09-02 at 4.06.07 PM.png" alt=""><figcaption><p>See details for a specific build.</p></figcaption></figure>

### Managing Flaky Behavior

One of the core workflows of FlakyBot is monitoring and augmenting CI runs. Once the CI is completed, the bot analyzes the results. The FlakyBot engine analyzes the CI runs on the golden branch to track flaky tests. If any test that is deemed flaky fails, there are a few ways to handle this:

* Each individual test can be set to rerun X number of times. In this case, these tests are automatically rerun the specified number of times to identify true failures. The actions can be configured on a per test basis. After reruns are complete, a final report is generated. If the test passed during a rerun, the final report will be reported as all clear. This can be used as a required check instead of the original CI run, in order to manage flaky behavior.

![FlakyBot reports can be used as a required check.](<../.gitbook/assets/Screen Shot 2021-09-04 at 8.07.20 PM.png>)

* If no reruns are configured, our FlakyBot engine compares the test logs. If the logs are identical to the ones reported in a flaky test, the failure is ignored. Like in the above scenario, the final report of FlakyBot calculates whether the status check should be accepted and reports that as a GitHub Check.

In addition, if a build is run twice on the same commit SHA of any branch, the inconsistencies in the runs are also used to identify flaky behavior.

### Infrastructure failures

In a CI environment, many times the entire infrastructure can fail. Examples include: docker failed to provision, failure to download a dependency or library, network issues, or system downtime. In such scenarios, there are no test-level insights but the entire test suite fails. FlakyBot also identifies these scenarios and ignores the test reports.

### Manual Adjustments

We understand that not everything that is observed and tracked by the core FlakyBot engine is perfect. Therefore, FlakyBot provides the ability to mark certain individual tests as flaky in the FlakyBot UI dashboard, as well as the ability to change the flakiness factor.

![Customize how you want to handle individual flaky tests.](<../.gitbook/assets/Screen Shot 2021-09-04 at 8.30.22 PM.png>)

### Other Configurations

#### Enable FlakyBot status check

This helps manage flaky test behavior mentioned above. You can add a FlakyBot status check and mark it as a required test in order to exclude flaky tests in your CI runs.

#### Nightly Jobs

You can specify a nightly job to run for a specific build. FlakyBot will rerun the last green build in order to more aggressively identify flaky tests.

<figure><img src="../.gitbook/assets/Screen Shot 2022-09-02 at 4.31.22 PM.png" alt=""><figcaption><p>Additional configurations.</p></figcaption></figure>

### Categorization

As our core engine learns over time, we will be able to categorize the Flaky tests by specific reason, and suggest fixes. Additionally, with semantic understanding of the codebase, FlakyBot can also stub out flaky tests and create tasks in an issue tracker for developers to fix. The goal is to transform flakiness into an automatic process to manage build health.

Want access to Flakybot? Please contact info@aviator.co.
