# Webhooks

Your application can use webhooks to subscribe to events happening on your Aviator account.

When configuring a webhook, you can use the UI to choose which event actions will send you payloads. Only subscribing to the specific events you plan on handling limits the number of HTTP requests to your server. You can also subscribe to all current and future event actions. You can change the list of subscribed events anytime.

Each event corresponds to a certain set of actions that can happen on your organization’s repository. For example, if you subscribe to the merge failure event you'll receive detailed payloads every time a PR fails to merge.

### Payloads

Each webhook event payload also contains properties unique to the event. Most of the common properties are listed below.

| Key                  | Description                                                                                                                                                                                                      |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **action**           | All webhook payloads contain an action property that contains the specific activity that triggered the event.                                                                                                    |
| **repository**       | Name of Github repository associated with the action.                                                                                                                                                            |
| **organization**     | Name of the Github organization associated with the action.                                                                                                                                                      |
| **pr\_number**       | _Integer_. PR Number associated with the action.                                                                                                                                                                 |
| **author**           | Github handle of the author of the PR.                                                                                                                                                                           |
| **status**           | Current status of the PR. Valid options: open, pending, queued, blocked, merged                                                                                                                                  |
| **skip\_line**       | _Boolean_. Represents whether the skip line label is present for the PR.                                                                                                                                         |
| **status\_code**     | Represents the reason for failure, if there was a failure. See [<mark style="color:blue;">Status Codes</mark>](comments-and-status-codes.md) <mark style="color:blue;"></mark> section for all possible options. |
| **message**          | _Optional_. Present if there is an additional message provided by Github on the reason for failure.                                                                                                              |
| **failed\_ci\_list** | _Optional_. _List_. List of CI names that failed in case of CI failure.                                                                                                                                          |

### Actions

Below is the list of actions that can be configured in the MQ UI to receive webhook events.

| Name               | When it is triggered                                                                                                                                    |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **opened**         | When the PR is opened.                                                                                                                                  |
| **labeled**        | When the Aviator trigger label was added to a PR.                                                                                                       |
| **queued**         | When the PR is queued. This typically happens when the PR is in an approved state and the Aviator trigger label is associated with the PR.              |
| **dequeued**       | When the Aviator label is manually removed from the PR. It is not reported in case of PR failing to merge.                                              |
| **top\_of\_queue** | When the PR reaches the top of the queue for processing.                                                                                                |
| **merged**         | When the PR is successfully merged.                                                                                                                     |
| **blocked**        | When the PR fails to merge and is blocked. The typical reason for failures can be retrieved from `status_code`, including CI failure or merge conflict. |
| **stuck**          | _(Parallel mode only)_ When a PR is stuck if the original PR is still running the checks after the specified timeout.                                   |
| **reset**          | _(Parallel mode only)_ When a parallel PR queue is reset.                                                                                               |

### Slack notifications <a href="#slack-notifications" id="slack-notifications"></a>

You can use the Slack app integration to receive notifications about queue events. Currently supported Slack events are queued, merged and failed. Slack notifications can be enabled from the Webhooks page.
