# SLO Management

With the objective of making code reviews faster, FlexReview SLO (Service Level Objective) standardizes the code review process for teams. With SLO:

* Teams can track SLOs and better understand the bottlenecks in code reviews
* Authors are incentivized to create better reviewable PRs
* Automated actions can be configured when SLOs are missed

## Review SLO

Review SLO is the suggested time it should take the reviewer to respond to a PR. Note that this is NOT the time it takes for a PR to be approved, but rather just getting **the first response** back.

Think of the code review SLO as an agreement between the author and the reviewer for a pull request. With this agreement, the author is incentivized to create PRs that falls within the bounds of the SLO agreement, and reviewer are incentivized to stay on top of the PRs.

## Considerations for Review SLO

There are a few things that FlexReview accounts for while calculating the SLO:

* Teams work in different timezones, or different business hours
* Code reviews can come in various sizes, a 2 lines review may be quick to review but a 1000 line change may take forever
* Reviews are generally faster within a team vs when dealing with external teams.

## FlexReview SLO

To build a meaningful SLO framework, FlexReview accounts for these factors. You can define code review first response times within the code size limits. For example, a code change with less than 400 lines should be reviewed within 1 business day. It tracks the timezones and business days to get a clear picture.

This incentivizes authors to create PRs that will fall into the SLO agreement (smaller changes), thereby also improving the ability to review the code, and the response times. And also incentivizes the reviewers to respond to PRs in time to avoid missing the timeline.

### Config

We will define the SLO specific config at two levels:

* Company level or the default config
* Overrides at a team level (maintaining level of hierarchy)

Although it is recommended that the companies should strive for a singular config (default config), but for large companies with mixed working styles, overrides may be necessary.

The config at each level include:

* Two separate SLO in hours - one for inter-team reviews and other for intra-team reviews
* Max number of lines of change for the SLO agreement to be valid

The SLO tracking is disabled when the change is larger than the max number of lines.

### 30 days analytics

SLO analytics helps you monitor the last 30 days PR activity for each repository. You can monitor separately both internal team and external team reviews. These SLO calculations are done based on business days. You can track:

* SLO hits / misses by team
* Show P50, P90, P99 for response times by team breakdown
* Identify non-eligible PRs - the PRs that exceed maximum modified lines specified in the SLO config.

### Actions

The automated actions are still under development and will be launched shortly as part of our roadmap.
