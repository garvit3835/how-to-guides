# FlakyBot

## Flaky tests

Flaky tests are non-deterministic tests that pass and fail without any code changes. It causes builds to fail even though the developer hasn’t made any related changes. This requires manual reruns of full CI that requires unnecessary developer overhead and increases in CI compute cost.

## Introducing FlakyBot

Aviator’s FlakyBot acts as a shock-absorber for flaky tests. It is an app that connects to your GitHub account and the CI provider (currently supports CircleCI, BuildKite, and Jenkins). It pulls the test artifacts for every CI run - these test artifacts are processed to identify each individual test status and reasons for failures. Based on the specified preconditions, Aviator’s FlakyBot will provide a custom report that can be used to pass / fail the builds.

FlakyBot also acts as a GitHub status check (CI check) on your PRs in addition to existing CI status checks. This FlakyBot CI check analyzes the CI test artifacts and produce a CI check report excluding flaky tests. This report can be used as a pass / fail requirement instead of the original CI status checks. The core FlakyBot engine uses a mix of heuristics and machine learning to flag each test result with root cause analysis.

The main goal of FlakyBot is to create automatic shock-absorbers for flaky tests while identifying true failures. As a result, developers do not have to worry about broken builds that are unrelated to his or her changes.

## Under the hood

### CI observation modes

#### **1. CI runs on the golden branch**

A golden branch is the protected base branch (typically referred to as `master` or `main`) that is assumed to be always passing. Any failures of CI on this branch are deemed flaky. Occasional true failures in this branch can be manually flagged as such to keep data coherent.

FlakyBot also reports the results of the CI checks here with a historical view, and provides stats around destabilization of builds.

![See your test history.](<../.gitbook/assets/Screen Shot 2021-09-04 at 7.10.52 PM.png>)

#### **2. CI run on a PR**

This is the core workflow of FlakyBot. These executions are monitored and augmented by FlakyBot. Once the CI is completed, the bot analyzes the results. The core FlakyBot engine uses the analysis from the CI runs on the golden branch to track flaky tests. If any test that is deemed flaky fails, there are a few ways to handle this:

* Each individual test can be set to rerun X number of times. In this case, these tests are automatically rerun the specified number of times to identify true failures. The actions can be configured on a per test basis. After reruns are complete, a final report is generated. If the test passed during a rerun, the final report will be reported as all clear. This can be used as a required check instead of the original CI run, in order to manage flaky behavior.

![FlakyBot reports can be used as a required check.](<../.gitbook/assets/Screen Shot 2021-09-04 at 8.07.20 PM.png>)

* If no reruns are configured, our FlakyBot engine compares the test logs. If the logs are identical to the ones reported in a flaky test, the failure is ignored. Like in the above scenario, the final report of FlakyBot calculates whether the status check should be accepted and reports that as a GitHub Check.

#### 3. Repeat CI runs on a commit SHA

Moreover, if a build is run twice on the same commit SHA of any branch, the inconsistencies in the runs are also used to identify flaky behavior.

### Infrastructure failures

In a CI environment, many times the entire infrastructure can fail. Examples include: docker failed to provision, failed to download a dependency or library, network issues, or system downtime. In such scenarios, there are no test-level insights but the entire test suite fails. FlakyBot also identifies these scenarios and reruns the entire suite.

### Manual Adjustments

We understand that not everything that is observed and tracked by the core FlakyBot engine is perfect. Therefore, FlakyBot provides the ability to mark certain individual tests as flaky in the FlakyBot UI dashboard, as well as the ability to change the flakiness factor.

![Customize how you want to handle individual flaky tests.](<../.gitbook/assets/Screen Shot 2021-09-04 at 8.30.22 PM.png>)

### Categorization

As our core engine learns over time, we will be able to categorize the Flaky tests by specific reason, and suggest fixes. Additionally, with semantic understanding of the codebase, FlakyBot can also stub out flaky tests and create tasks in an issue tracker for developers to fix. The goal is to transform flakiness into an automatic process to manage build health.

Want access to Flakybot? Please contact support@aviator.co.
