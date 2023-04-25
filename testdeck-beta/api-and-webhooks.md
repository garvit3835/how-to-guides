# API and webhooks

## API

{% swagger method="get" path="/flaky" baseUrl="https://api.aviator.co/api/v1/testdeck" summary="Get flaky tests in the last 30 days." expanded="true" %}
{% swagger-description %}
The flaky test API can be used to fetch the results of all flaky tests in the last 30 days. The results are paginated with maximum of 100 results returned in the response. The results are in the order of the most recent flaky, based on 

`first_occurrence`

.
{% endswagger-description %}

{% swagger-parameter in="body" name="org" type="String" required="true" %}
Organization name for the repo.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="repo" type="String" required="true" %}
Repository name.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="github_check_name" type="String" required="true" %}
Name of the GitHub check
{% endswagger-parameter %}

{% swagger-parameter in="body" name="page" type="Integer" %}
Page number
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
  "repository": {
     "name": "repo_name",
     "org": "repo_org"
  },
  "github_check": {
    "name": "ci/circleci: unit-tests",
    "provider": "CircleCI"
  },
  "test_cases": [
    {
      "name": "test_invalid_security_code",
      "class_name": "core.payments.SecurityCodeTest",
      "first_occurrence": "2022-11-16T17:21:41.350499",
      "last_occurrence": "2022-11-18T11:13:23.12361",
      "details_url": "https://app.aviator.co/flaky/test_case/101063",
      "last_24_hours_count": 5
    },
    {
      "name": "test_valid_security_code",
      "class_name": "core.payments.ValidCodeTest",
      ...
    },
    ..
  ]
}
```
{% endswagger-response %}
{% endswagger %}

## Webhooks

Aviator already supports a [<mark style="color:blue;">webhook framework</mark>](https://docs.aviator.co/reference/webhooks) that lets you listen to various events. By using Aviator’s new capabilities to detect flaky tests, you can now subscribe to these flaky test webhooks to take actions based on the event payload.

### Event payload

Example JSON payload for TestDeck webhook:

```json
{
  "action": "new_flaky_test",
  "repository": {
     "name": "repo_name",
     "org": "repo_org"
  },
  "test_suite": {
    "name": "ci/circleci: unit-tests",
    "provider": "CircleCI"
  },
  "test_case": {
    "name": "test_invalid_security_code",
    "class_name": "core.payments.SecurityCodeTest",
    "first_occurrence": "2022-11-16T17:21:41.350499",
    "last_occurrence": "2022-11-18T11:13:23.12361",
    "last_24_hours_count": 5
  },
  "commit_sha": "c576a61d26f72ad335660bdaefa4d63306c5752c"
}
```

Note: `commit_sha` is an optional field and will only be present when notifying about a `new_flaky_test`.

### Event actions

You can subscribe to these specific actions for webhooks.

* `new_flaky_test` - a new flaky test is found.
* `resolved_flaky_test` - when an existing flaky test has not been identified as flaky in the last 14 days
