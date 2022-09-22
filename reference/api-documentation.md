# API Documentation

## **Authentication**

Aviator uses basic key based authentication. You can view and manage your API key in the [<mark style="color:blue;">Aviator dashboard</mark>](https://mergequeue.com/account/api). We only support a single API token. Once a token is generated, you can reset it to invalidate your existing token and create a new one.

API keys that are used to fetch Aviator analytics are currently read-only, but in the future these may be used for other purposes. Please make sure to keep them secure.

Authentication to the API is performed via [<mark style="color:blue;">HTTP Basic Auth</mark>](http://en.wikipedia.org/wiki/Basic\_access\_authentication)<mark style="color:blue;">.</mark> Provide your API key as the bearer token:

```shell
curl -H "Authorization: Bearer <aviator_token>"
-H "Content-Type: application/json"
https://api.aviator.co/api/v1/curl -H "Authorization: Bearer <aviator_token>" \\ -H "Content-Type: application/json" \\ <https://mergequeue.com/api/v1/>
```

## API Objects

### Repository

{% swagger method="post" path="/v1/repo" baseUrl="https://api.aviator.co/api" summary="Pause / unpause the Aviator bot processing on a repository." %}
{% swagger-description %}
Example:

`curl -X POST -H "Authorization: Bearer <aviator_token>"`\
`-H "Content-Type: application/json"`\
`-d '{"org": "org_name", "name": "repo_name", "paused": true }'`\
`https://api.aviator.co/api/v1/repo/`
{% endswagger-description %}

{% swagger-parameter in="body" name="org" type="String" required="true" %}
Name of the Github organization.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="String" required="true" %}
Name of the repository in the Github organization.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="paused" type="Boolean" required="true" %}
Whether to pause or unpause the queue
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```javascript
{
  "org": "ankitjaindce",
  "name": "testrepo",
  "paused": false
}
```
{% endswagger-response %}
{% endswagger %}

### PullRequest

{% swagger method="post" path="v1/pull_request" baseUrl="https://api.aviator.co/api/" summary="Queue or Dequeue a Pull Request" %}
{% swagger-description %}
`curl -X POST -H "Authorization: Bearer <aviator_token>"`

\


`-H "Content-Type: application/json"`

\


`-d '{"action": "queue", "pull_request": {"number": 1234, "repository": {"name": "repo_name", "org": "org_name"}, "affected_targets": ["targetA", "targetB"], "merge_commit_message": {"title": "This is where title goes", "body": "This is where body goes"}}}'`

\


`https://mergequeue.com/api/v1/pull_request/`
{% endswagger-description %}

{% swagger-parameter in="body" name="action" required="true" %}
Action taken. Valid options: 

`queue`

 or 

`dequeue`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="pull_request" type="Object" required="true" %}
PullRequest object representing the PR that is queued.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="> number" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="> repository" type="Object" required="true" %}
Repository object associated with the PR
{% endswagger-parameter %}

{% swagger-parameter in="body" name="> > name" required="true" %}
Name of the repository
{% endswagger-parameter %}

{% swagger-parameter in="body" name="> > org" required="true" %}
Organization associated with the repository
{% endswagger-parameter %}

{% swagger-parameter in="body" name="> affected_targets" type="List[String]" %}
Affected targets for the PR. Please see 

[Affected Targets](../how-to-guides/affected-targets.md)

 section for more details.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="> merge_commit_message" type="Object" %}
CommitMessage object
{% endswagger-parameter %}

{% swagger-parameter in="body" name="> > title" %}
Title of merge commit message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="> > body" %}
Body of merge commit message
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "pull_request": {
    "number": 1234,
    "status": "queued",
    "queued": true,
    "title": "[TASK-123] This is a new bug",
    "author": "av_user",
    "head_branch": "av/new_fix",
    "base_branch": "main",
    "repository": {
      "name": "av-demo",
      "org": "aviator-co"
    }
  }
}
```
{% endswagger-response %}
{% endswagger %}

### Analytics

{% swagger method="get" path="/v1/analytics" baseUrl="https://api.aviator.co/api" summary="Get list of analytics objects representing statistics on a daily basis." %}
{% swagger-description %}
Example:

`curl -H "Authorization: Bearer <aviator_token>"`\
`-H "Content-Type: application/json"`\
`https://api.aviator.co/api/v1/analytics/?start=2021-07-14&end=2021-07-21&timezone=America/Los_Angeles&repo=orgname/reponame`
{% endswagger-description %}

{% swagger-parameter in="query" name="start" required="false" type="String" %}
UTC Start date in 

_YYYY-MM-DD_

 format. Example: 2021-07-21
{% endswagger-parameter %}

{% swagger-parameter in="query" name="end" required="false" type="String" %}
UTC End date in 

_YYYY-MM-DD_

 format. Example: 2021-07-21
{% endswagger-parameter %}

{% swagger-parameter in="query" name="repo" required="true" type="String" %}
Name of the Github repo, in the format: 

_orgname/reponame_
{% endswagger-parameter %}

{% swagger-parameter in="query" name="timezone" type="String" %}
Standard tz format string. Defaults to account timezone. Example: America/Los_Angeles
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
{% tabs %}
{% tab title="Parameters" %}
| Parameter             | Description                                                                                                 |
| --------------------- | ----------------------------------------------------------------------------------------------------------- |
| **time\_in\_queue**   | List of objects representing time spent by PRs in queue                                                     |
| **>date**             | UTC End date in _YYYY-MM-DD_ format. Example: 2021-07-14                                                    |
| **>min**              | Minimum time in seconds                                                                                     |
| **>avg**              | Average time in seconds                                                                                     |
| **>p50**              | 50th percentile in seconds                                                                                  |
| **>p75**              | 75th percentile in seconds                                                                                  |
| **>p90**              | 90th percentile in seconds                                                                                  |
| **>p100**             | 100th percentile in seconds                                                                                 |
| **mergequeue\_usage** | List of objects representing the Aviator bot usage compared to total PRs merged.                            |
| **>date**             | UTC End date in _YYYY-MM-DD_ format. Example: 2021-07-14                                                    |
| **>total**            | Total number of PRs merged                                                                                  |
| **>merged\_by\_bot**  | Total number of PRs merged by Aviator bot                                                                   |
| **blocked\_reason**   | List of objects representing the blocked reasons identified by the Aviator bot while processing queued PRs. |
| **>date**             | UTC End date in _YYYY-MM-DD_ format. Example: 2021-07-14                                                    |
| **>merge\_conflict**  | Failed due to merge conflict                                                                                |
| **>ci\_failure**      | Failed due to CI status check failure. This only accounts for required check failures.                      |
| **>manual\_dequeue**  | A PR was manually removed from the queue                                                                    |
| **>ci\_timeout**      | CI timed out based on the configuration in MergeQueue rules                                                 |
| **>other**            | Failed due to any other reason                                                                              |
| **sync\_frequency**   | List of objects representing how many times a PR fetched a base branch on an aggregate basis.               |
| **>date**             | UTC End date in _YYYY-MM-DD_ format. Example: 2021-07-14                                                    |
| **>min**              | Minimum sync times                                                                                          |
| **>avg**              | Average sync times                                                                                          |
| **>p50**              | 50th percentile of number of sync times                                                                     |
| **>p75**              | 75th percentile of number of sync times                                                                     |
| **>p90**              | 90th percentile of number of sync times                                                                     |
| **>p100**             | 100th percentile of number of sync times                                                                    |
{% endtab %}

{% tab title="Example" %}
```javascript
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
{% endtab %}
{% endtabs %}
{% endswagger-response %}
{% endswagger %}
