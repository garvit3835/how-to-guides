# Configuration file

Aviator supports a YAML based configuration file. This file can either be applied directly from the Aviator dashboard or configured in the GitHub repository.

### From Dashboard

You can directly apply the config on the _Yaml configuration_ tab on the[ <mark style="color:blue;">Merge Rules</mark>](https://app.aviator.co/github/rules) page. You can also validate this configuration before applying any changes.

### From GitHub repository

You can also create a configuration file stored in `.aviator/config.yml`. The file will only be read once it is merged into the repository's default branch. It will also override any properties set in the Dashboard UI.

## Examples

### Minimalist config

The only required attribute is `merge_rules.labels.trigger`.

```yaml
merge_rules:
  labels:
    trigger: "mergequeue"
```

### Custom required checks

Checkout customizing required checks section for details.

```yaml
merge_rules:
  labels:
    trigger: "mergequeue"
  preconditions:
    use_github_mergeability: false
    required_checks:
      - build
      - unit-test
      - typecheck
      - ci/circleci: pytest

```

### Require all conversation resolution

This enforces all conversations in GitHub to be resolved before the PR can be merged.

```yaml
merge_rules:
  labels:
    trigger: "mergequeue"
  preconditions:
    conversation_resolution_required: true
```

### No approval

Useful for testing. Aviator will only be able to merge PR if the approval is not enforced at GitHub level. By default, Aviator always require approvals.

```yaml
merge_rules:
  labels:
    trigger: "mergequeue"
  preconditions:
    number_of_approvals: 0
```

### Using Parallel mode

Also checkout the [<mark style="color:blue;">parallel mode section</mark>](../parallel-mode/) for details.

```yaml
 merge_rules:
   labels:
     trigger: "mergequeue"
   merge_mode:
    type: "parallel"
    parallel_mode:
      max_parallel_builds: 10
      override_required_checks:
        - build_and_test
        - 'ci/circleci: build'
```

### Automatic requeue

On failure, the PRs will automatically requeue before giving up. Only available in parallel mode.

```yaml
 merge_rules:
   labels:
     trigger: "mergequeue"
   merge_mode:
    type: "parallel"
    parallel_mode:
      max_parallel_builds: 10
      max_requeue_attempts: 3
```

### Auto update

Keep your PRs up to date. Every time a new. commit is added to the base branch, the PRs are automatically updated using rebase or merge commit. See the [<mark style="color:blue;">auto update section</mark>](complete-reference-guide.md#auto-update) in the reference guide.

```yaml
 merge_rules:
   labels:
     trigger: "mergequeue"
   auto_update:
     enabled: false
     label: "auto_update"
     max_runs_for_update: 10
```

### Custom title and body

Customize title and body when merging the PR.

```
 merge_rules:
   labels:
     trigger: "mergequeue"
   merge_commit:
     use_title_and_body: true
     cut_body_before: "----"
     cut_body_after: "+++"
```
