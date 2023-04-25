# Analytics

Analytics offers insights into the performance of Aviator and provides metrics on its usage. The following metrics are offered on the Analytics page of Aviator.

### **Time in Queue (mins)**

The time a PR spends in the queue before it is merged by the Aviator bot. The time is measured starting when the PR is queued (not labeled). This only includes the PRs that were merged by the Aviator bot. The metric is reported as: Average, p50 (50th percentile), p75 (75th percentile), p90 (90th percentile) and p100 (100th percentile).

![](<../.gitbook/assets/Screen Shot 2022-05-17 at 3.36.03 PM.png>)

### Aviator **bot usage**

Represents how many PRs were merged by the bot vs. merged manually segmented by day.

### **PR failure reasons**

Gives you a better understanding of how many PRs are failing to automatically merge and why. The breakdown is done by Failure reasons. You can find more details in the [<mark style="color:blue;">Error Codes</mark>](comments-and-status-codes.md) section.

The failed PR is counted multiple times if it failed multiple times in its lifecycle.

### **PR sync frequency**

This metric captures how many times the bot pulls the latest base branch into the current PR. This should only happen at most once if everyone uses the Aviator bot for merging the PRs.

## API

Analytics are always available using the `/analytics/` endpoint:

```
curl -H "Authorization: Bearer <aviator_token>" \
-H "Content-Type: application/json" \
https://api.aviator.co/api/v1/analytics/?start=2021-07-14&end=2021-07-21&timezone=America/Los_Angeles&repo=orgname/reponame
```

**Response**

```
{
    "time_in_queue" : [
      {
        "date": "2021-07-14",
        "min": 12,
        "avg": 24,
        "p50": 23,
        "p75": 28,
        "p90": 32,
        "p100": 40
      },
      {
        "date": "2021-07-15",
        "min": 12,
        "avg": 24,
        "p50": 23,
        "p75": 28,
        "p90": 32,
        "p100": 40
      },
      {...},
      {...}
    ],
    "mergequeue_usage": [
      {
        "date": "2021-07-14",
        "total": 52,
        "merged_by_bot": 40
      },
      {
        "date": "2021-07-15",
        "total": 52,
        "merged_by_bot": 40
      },
      {...},
      {...}
    ],
    "blocked_reason": [
      {
        "date": "2021-07-14",
        "merge_conflict": 2,
        "ci_failure": 4,
        "manual_dequeue": 6,
        "ci_timeout": 1,
        "other": 0
      },
      {
        "date": "2021-07-15",
        "merge_conflict": 2,
        "ci_failure": 4,
        "manual_dequeue": 6,
        "ci_timeout": 1,
        "other": 0
      },
      {...},
      {...}
    ],
    "sync_frequency": [
      {
        "date": "2021-07-14",
        "min": 1,
        "avg": 1,
       "p50": 2.34,
        "p75": 2.6,
        "p90": 3.2,
        "p100": 4
      },
      {
        "date": "2021-07-15",
        "min": 1,
        "avg": 1.2,
        "p50": 2.34,
        "p75": 2.6,
        "p90": 3.2,
        "p100": 4
      },
      {...},
      {...}
    ]
  }
```

Also see API specs in the [analytics section](../api-documentation.md#analytics) of the API documentation.
