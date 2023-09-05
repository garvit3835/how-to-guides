# Personal Integrations

## GitHub Integration

On the Personal Integrations page, each user can connect their GitHub account.

<div data-full-width="false">

<figure><img src="../.gitbook/assets/Screen Shot 2023-08-20 at 6.13.08 PM.png" alt=""><figcaption></figcaption></figure>

</div>

Once connected, your team can write [custom Pilot workflows ](../pilot-automated-actions.md#slack)for specific GitHub users.

## Slack Integration

### DM Notifications

You can link your Slack account to your Aviator account. The Aviator Slack app will send direct messages about your own PR activity. Simply use the slash command `/aviator connect` and login to complete the integration.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Follow link to connect your Slack account.</p></figcaption></figure>

Once completed, you'll receive DMs about your PR activity.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Notifications for your PR activity.</p></figcaption></figure>

### Personalized DMs (Opt-in or Opt-out)

With Pilot's Direct Message Action, you can send a DM to a specific user or group based on a trigger. Each user can further customize which DMs to receive based on the Action's labels.

1. Make sure that you have connected both [GitHub](https://app.aviator.co/integrations/personal) and [Slack](personal-integrations.md#dm-notifications) accounts.
2. Configure a Pilot scenario that uses the Slack Direct Message action. See below for both opt-out and opt-in examples.

```yaml
scenarios:
  - name: "Queued PRs (Opt-out example)"
    trigger: 
      mergequeue: queued
    actions:
      - slack:
          direct:
            text: “A new PR has been queued.”
            github_users:
              - login: test_user1
              - login: test_user2
            labels:
              - queued-pr
  - name: "Merged PRs (Opt-in example)"
    trigger: 
      mergequeue: merged
    actions:
      - slack:
          direct:
            text: “A PR has been merged.”
            github_group: "engineering"
            labels:
              - merged-pr
            opt-in: True
```

3. For the Queued PRs trigger, by default, both `test_user1` and `test_user2` will receive DMs when a PR is queued. In order to opt-out of the DM, you can add the `queued-pr` label to your opt-out list shown below.\
   \
   For the Merged PRs trigger, by default, no users will receive DMs since the action requires users to opt-in. Anyone within the `engineering` GitHub group can opt-in by adding the `merged-pr` label to their opt-in list as shown below.

<figure><img src="../.gitbook/assets/Screen Shot 2023-09-01 at 3.52.20 PM.png" alt=""><figcaption><p>Personalize individual DMs with opt-out or opt-in labels.</p></figcaption></figure>

