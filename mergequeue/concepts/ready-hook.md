# Ready Hook

MergeQueue supports user-defined _ready hooks_ which can execute custom
JavaScript code to finely control how MergeQueue behaves.

The ready hook is executed once whenever a pull request is marked as
ready-to-merge (usually via adding the repository's configured label, by using a
[slash command](/mergequeue/slash-commands.md), or using the
[Aviator API](/api/README.md)).

The ready hook is defined by creating a file in your GitHub repository at
`.aviator/mergequeue/ready.js` and should define a function `ready` that will be
called by the
[Pilot JavaScript runtime](/pilot-automated-actions/js-execution.md).

## Example: Ensuring pull request freshness

Pull requests that are out of date are likely to have merge conflicts and/or
cause failures once they enter the queue which can degrade the overall
throughput and time-to-merge latency of MergeQueue.

Using the ready hook, we can ensure that pull requests are up-to-date before
they enter the queue.

```js
// .aviator/mergequeue/ready.js

// We'll only update the pull request if it's more than 50 commits behind.
const MAX_COMMITS_BEHIND = 50;

function ready() {
  const base = $event.pullRequest.base.ref;
  const head = $event.pullRequest.head.ref;
  const { behindBy } = $github.compareCommits({ base, head });
  if (behindBy > MAX_COMMITS_BEHIND) {
    $github.addComment(
      `Pull request is ${behindBy} commits behind ${base}.` +
      `Synchronizing pull request with latest commits before queuing.`
    );

    // Update the pull request with the latest commit from its base branch using
    // the repository's configured strategy (rebase or merge).
    $mergequeue.synchronizePullRequest();
  }
}
```
