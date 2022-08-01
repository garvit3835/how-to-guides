---
description: Merge dependent PRs together
---

# ChangeSets

The ChangeSets feature allows you to create a list of PRs to be merged together. This is useful if you have a large number of PRs that need to be merged together (e.g., coordinated changes to a web frontend, iOS app, and Android app). ChangeSets can group PRs within the same repository or across multiple repositories.

You can create a ChangeSet by clicking the "Create New" button on the ChangeSet page. This will create an empty ChangeSet.

Add a PR to this ChangeSet by finding the PR you want to add (search by Github user, or by Repository name), and click the "Add to ChangeSet" button next to it. Repeat the process to add as many PRs as you like to this ChangeSet.

To remove a PR from a ChangeSet, click the "Remove" button next to the PR name.

Once ready, you can merge the ChangeSet by clicking the "Merge" button on the page. Aviator will then validate the approvals and status checks for all the PRs in the ChangeSet and merge them together. You can optionally specify additional validation requirements that must be met before the whole ChangeSet is merged. If validation fails, the ChangeSet will be marked as failed and none of the PRs will be merged.

## Configuring ChangeSets

### Step 1: Connect your repositories to Aviator

If you haven't connected your GitHub repositories to Aviator yet, you can do so by clicking the "Add Repos" button on the [Repositories page](https://mergequeue.com/github/repos).

### Step 2: Configure ChangeSet settings

There are a few settings you can configure on the [ChangeSet settings page](https://mergequeue.com/changeset/settings):

* **Enable comments for new PRs** - When this setting is turned ON, Aviator will post a comment on each PR to link it with a new or existing ChangeSet.
* **Require global CI validation** - Continue to [global CI validation](global-ci-validation.md) for more details.

### Step 3: Configure webhooks

If you are using global CI validation, you must also make sure the webhook is configured. See the [Webhooks](../../reference/webhooks.md) section to configure your webhooks.&#x20;

