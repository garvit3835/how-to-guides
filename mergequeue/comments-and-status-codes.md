# Comments and Status Codes

Comments in PRs are a great way to stay informed of the activity of Aviator bot. Comments are enabled by default, and can be disabled from the Merge Rules page.&#x20;

![Aviator bot will post comments with information about failed builds.](<../.gitbook/assets/Screen Shot 2022-05-23 at 5.33.50 PM.png>)

The following comments are generated when this feature is enabled -

* **CI is taking longer than expected.** - This is only applicable in Parallel mode. It can happen if the draft PR CI is completed but the original PR is still pending CI status. If the PR is `stuck`, then the comment will also provide info on when the PR will be dequeued and which CI(s) are stuck.
* **This PR is queued for merge (queue position: \<number>).** - Reports successful queuing along with the current position of the PR in the queue. Info on pending status checks will also be provided.
* **PR failed to merge with reason: \<reason>** - Reports a PR getting dequeued with one of the reasons as described above. It may also contained information about the associated draft PR if relevant along with additional debug info.
* **CI for this PR is waiting to be queued \<note>. CI for this PR will run automatically when the queue catches up.**
* **This PR was merged using MergeQueue.**
* **This PR was merged manually.**
* **This PR was closed without merging. If you still want to merge this PR, re-open it.**
* **This PR is not ready to merge (currently in state \<status>: \<description>).**

#### Sticky Comments

Some of the comments explained above and below are posted as sticky comments - a single comment on a PR that is edited as the status of the PR changes. The goal is to keep the comments relevant and reduce noise on the PR.

![The sticky comment posted when the trigger label is added.](<../.gitbook/assets/Screen Shot 2022-05-23 at 5.37.18 PM.png>)

Once the PR is merged, Aviator will update the comment.

![Updated sticky comment when the PR is merged.](<../.gitbook/assets/Screen Shot 2022-05-23 at 5.32.41 PM.png>)

#### Stacked PRs

The following comments are only applicable if you are using Stacked PRs.

* **Waiting for the previous PR in this stack to be queued (or merged) before queuing this PR.** - This is only applicable if using stacked PRs.
* **This PR is stacked on top of \<PR#>. Changes from this PR can be reviewed separately and changes to the parent PR will automatically be synced into this PR.**
* **Next PR in this stack: \<dependent PR#s>**

#### ChangeSets

The following comments are only applicable if you are using ChangeSets.

* **PR has been added to the ChangeSet.**
* **MergeQueue ChangeSet actions: \[View Changeset dashboard]\<dashboard\_url>**

### Status Codes

If Aviator runs in any issues or failures, we will surface a status code or its corresponding description. Please refer to the table below.

| Status                      | Code | Description                                                                                                                                                                                                                                                                                                                                            |
| --------------------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| NOT\_APPROVED               | 101  | PR does not have the required number of approvals.                                                                                                                                                                                                                                                                                                     |
| READY\_LABEL\_REMOVED       | 104  | Someone has removed the trigger label to dequeue the PR.                                                                                                                                                                                                                                                                                               |
| BLOCKED\_BY\_LABEL          | 105  | Someone has manually added the failure label to block the PR.                                                                                                                                                                                                                                                                                          |
| MERGE\_CONFLICT             | 106  | There is a merge conflict.                                                                                                                                                                                                                                                                                                                             |
| FAILED\_TESTS               | 107  | One of more of the required CI status checks failed.                                                                                                                                                                                                                                                                                                   |
| MISSING\_OWNER\_REVIEW      | 109  | The PR does not have required reviews from the codeowners.                                                                                                                                                                                                                                                                                             |
| CI\_TIMEDOUT                | 111  | This is only reported if a CI timeout is specified in the merge rules. The PR's required CI status check did not complete in the specified time and will be dequeued.                                                                                                                                                                                  |
| STUCK\_TIMEDOUT             | 112  | The CI has already timed out, and the PR is still in a stuck state.                                                                                                                                                                                                                                                                                    |
| UNRESOLVED\_CONVERSATIONS   | 113  | The PR still has unresolved conversations. This is only applicable if `conversation_resolution_required=true` in merge rules.                                                                                                                                                                                                                          |
| DRAFT\_PRS\_UNSUPPORTED     | 114  | Draft PRs are only supported with a GitHub Team plan, GitHub Enterprise, or GitHub Enterprise Cloud. See the [<mark style="color:blue;">GitHub Docs</mark>](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests#draft-pull-requests) for more information. |
| UNVERIFIED\_COMMITS         | 115  | Not all commits in the PR have been signed/verified. This is applicable if your merge rules configuration requires verified commits.                                                                                                                                                                                                                   |
| COMMIT\_ADDED               | 116  | The PR is now stale because a new commit has been added since the CI has been run.                                                                                                                                                                                                                                                                     |
| CHANGES\_REQUESTED          | 117  | There are changes requested on the PR.                                                                                                                                                                                                                                                                                                                 |
| UNSUPPORTED\_BASE\_BRANCH   | 118  | We do not support merging into the specified base branch.                                                                                                                                                                                                                                                                                              |
| BLOCKED\_BY\_GITHUB         | 110  | Blocked due to internal GitHub reasons.                                                                                                                                                                                                                                                                                                                |
| FAILED\_UNKNOWN             | 108  | Failed due to an internal error.                                                                                                                                                                                                                                                                                                                       |
| MANUAL\_RESET               | 300  | Reset triggered manually.                                                                                                                                                                                                                                                                                                                              |
| RESET\_BY\_FAILURE          | 301  | A reset was triggered due to failure of CI                                                                                                                                                                                                                                                                                                             |
| RESET\_BY\_SKIP             | 302  |  A reset was triggered due to a new skip line PR                                                                                                                                                                                                                                                                                                       |
| RESET\_BY\_DEQUEUE          | 303  | A reset was triggered due to a PR being manually dequeued                                                                                                                                                                                                                                                                                              |
| RESET\_BY\_MANUALLY\_CLOSED | 304  | A reset was triggered because a PR was manually closed                                                                                                                                                                                                                                                                                                 |
| RESET\_BY\_FF\_FAILURE      | 305  | A reset was triggered because Aviator failed to fast forward                                                                                                                                                                                                                                                                                           |
| RESET\_BY\_CONFIG\_CHANGE   | 307  | A reset was triggered due to config change                                                                                                                                                                                                                                                                                                             |
| RESET\_BY\_TIMEOUT          | 308  | A reset was triggered due to timeout                                                                                                                                                                                                                                                                                                                   |