# Pilot JavaScript Execution Environment

Aviator has support for executing custom JavaScript code using
[Pilot automated actions](/pilot-automated-actions) or the
[MergeQueue ready hook](/mergequeue/concepts/ready-hook.md).

For security and sandboxing purposes, The Pilot JavaScript execution does not
provide most standard web APIs (such as `fetch`). Instead, several integrations
with [MergeQueue](/pilot-automated-actions/reference/mergequeue.md),
[GitHub](/pilot-automated-actions/reference/github.md), and
[Slack](/pilot-automated-actions/reference/slack.md) are provided.
