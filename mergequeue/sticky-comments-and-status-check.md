# Sticky comments and status check

In addition to the Aviator’s dashboard, Aviator provides two ways to give you the status of a PR within GitHub.

## Sticky comment

Aviator posts a comment when a PR is opened, and will automatically update the comment as the PR moves through its lifecycle. This is a great way to stay up to date and identify any reasons that may have dequeued your PR or is preventing your PR from merging.

<figure><img src="../../.gitbook/assets/Screen Shot 2023-04-24 at 12.31.23 PM (1).png" alt=""><figcaption></figcaption></figure>

You can also customize the sticky comment to show up when the PR is created, when it’s ready for review, or when it’s queued. You can also add your custom messages that could be helpful to link your own FAQs or self help documents for your team. You can read the full documentation here. Here’s a sample config

```
merge_rules:
  status_comment:
    # Optional. Valid values are "always", "queued", or "never".
    # Default value is "always".
    publish: "always"
    # A message to include when the pull request is in the open state.
    open_message: "..."
    # A message to include when the pull request is in the queued state.
    queued_message: "..."
    # A message to include when the pull request is in the blocked state.
    blocked_message: "..."

```

Having a sticky comment makes it easy for developers to find the status of the PR in one place, and also reduces noise if you set up notifications for new GitHub comments.

## Status check

Similar to the sticky comment, Aviator also provides a status check that you can enable for your PRs. You can also add this status check as a required check in branch protection rules to prevent developers from bypassing the merge queue. In the branch protection settings, you can find this check with the name: aviator/checks

<figure><img src="../../.gitbook/assets/Screen Shot 2023-04-20 at 9.51.52 AM.png" alt=""><figcaption></figcaption></figure>

The check typically contains similar details that you also see on the sticky comment.

<figure><img src="../../.gitbook/assets/Screen Shot 2023-04-20 at 9.51.07 AM.png" alt=""><figcaption></figcaption></figure>

This check is enabled by default but it can be disabled using a config setting:

```
merge_rules:
  publish_status_check: false
```
