# Slack notifications

You can also configure a daily summary to be reported via Slack for specific test suites. This requires the [<mark style="color:blue;">Slack integration</mark>](https://docs.aviator.co/reference/slack-integration) to be active. The Slack summary can be enabled via the YAML configuration file:

```
testdeck:
  jobs:
    - name: buildkite/pytest
      daily_summary:
        publish_time: "14:00" # UTC time as HH:MM
```

The daily summary includes:

* Total number of flaky runs today
* Most flaky test case of the day
* Any new flaky tests introduced in the last 24 hours
