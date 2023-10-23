# CI Status Requirements

Aviator MergeQueue supports any CI system that integrates with [GitHub status checks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/collaborating-on-repositories-with-code-quality-features/about-status-checks).

In GitHub, you can require tests to be passing with [protected branches require status check](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches#require-status-checks-before-merging) before merging. By default, Aviator MergeQueue uses the same data as GitHub status checks, but it allows you to add more requirements at different levels.

* Pre-queue status requirements
  * Same as the GitHub protected branch status requirement, this configuration is the status requirement before merging.
  * MergeQueue requires both GitHub protected branch configuration and this configuration to be satisfied. However, MergeQueue configuration can be more flexible than GitHub one.
* Draft PR status requirements
  * When a pull-request enters a queue, MergeQueue creates a new temporary branch and a PR, which is called "draft PR". This draft PR contains the changes from the queued PR and the latest target branch (e.g. `main`) so that we can test with the latest changes.

You can require the same sets of tests for both, or if needed, you can require different tests to be passing.

## See also

* [MQ Created Branches](optimizing-ci-execution.md) for additional details on the CI setup.
* [Pre-Queue Conditions](pre-queue-conditions.md) for other conditions you can set for the pre-queue time.
* [Conditional Checks](../how-to-guides/customize-required-checks.md#conditional-checks) and [wildcards](../how-to-guides/customize-required-checks.md#using-wildcards) for the flexible configuration options on the test status requirements.&#x20;
