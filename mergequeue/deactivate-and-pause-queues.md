# Deactivate and pause queues

Aviator provides two ways to temporarily stop the queue: deactivate and pause.

## Deactivate

A repository can be deactivated from the [<mark style="color:blue;">repositories page</mark>](https://app.aviator.co/github/repos). When a repository is deactivated, Aviator will still capture the webhooks to monitor the state of the PRs, but will not take any action or post any comments. You should use deactivate when you want to stop all Aviator activity. The only exception to this is that Changesets can still be created in a deactivated repository.

## Pause

Pause is a way to pause the queue or just the merge action. When a repository is active but paused, Aviator will continue to process and manage the queue. So, in a parallel mode queue, Aviator will also continue creating the parallel draft PRs, but it will not merge any PRs. This also includes the instant merge action. There are two levels of pausing the queue:

### Repository

You can pause all PRs in a repository. A repository can be paused from the [repositories page](https://app.aviator.co/github/repos) or using the API:

```bash
curl -X POST \
  -H "Authorization: Bearer <aviator_token>" \
  -H "Content-Type: application/json" \
  -d '{"org": "org_name", "name": "repo_name", "paused": true }' \
https://api.aviator.co/api/v1/repo/
```

### Base branch

Pausing the queue associated with a base branch can be helpful if you want to stop all merges while a release is being prepared. In this case, PRs that target all other base branches will continue merging regularly but the PRs associated with the specific base branch will be paused. Base branch pausing capability is only supported via the API:

```bash
curl -X POST \
    -H "Authorization: Bearer <aviator_token>" \
    -H "Content-Type: application/json" \
    -d '{ "pattern": "release-*", "repository": {"org": "aviator", "name": "av-demo-release"}, "paused": true, "paused_message": "This release branch has been paused."}' \
https://api.aviator.co/api/v1/branches
```

The base branch API also takes an optional parameter called paused\_message that provides an additional message in the GitHub comments for a PR when a queue is paused.

```bash
You can also use a GET request to find the current pause status of all branches:
curl -H "Authorization: Bearer <aviator_token>" \
    -H "Content-Type: application/json" \
    -d '{ "repository": {"org": "aviator", "name": "av-demo-release"}}' \
https://api.aviator.co/api/v1/branches
```
