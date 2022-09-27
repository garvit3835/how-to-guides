# Merge Rules

Merge rules are at the core of how the Aviator bot behaves. MergeQueue primarily relies on GitHub Labels to communicate with pull requests. There are several attributes that you can customize for each repository using a yaml configuration file.

### Option 1: Dashboard UI

The best option for a quick setup is via the [<mark style="color:blue;">Merge Rules</mark>](https://app.aviator.co/github/rules) page. However, the UI does not expose all possible customizations.

{% hint style="warning" %}
Some of the Rules listed in the yaml configuration below are not supported in the Dashboard UI. Your repo will require using one of the configuration file options if you want to use advanced rules. We only recommend using the Dashboard UI option if you require a simple setup.
{% endhint %}

The only required setting is the `Label for trigger` -  this will default to `mergequeue`. Once you add this to a PR, Aviator will queue and merge the PR.

![Label for trigger is the only required setting.](<../../.gitbook/assets/Screen Shot 2022-05-23 at 5.43.35 PM.png>)

All of the settings in the UI are covered in the [<mark style="color:blue;">Rules</mark>](attributes-and-examples.md) section. Each setting also has a tooltip that provides more information.

#### Configuring Status Checks

See `Required Checks` under `Merge Preconditions` to set status checks for your PRs that need to be validated before merging. Selecting `Use Github mergeability check` will use all Github required tests for the repo.

![Select Required status checks.](<../../.gitbook/assets/Screen Shot 2022-07-13 at 4.34.12 PM.png>)

If your team is using Parallel mode, by default, Aviator bot will use the same checks for original PRs and draft PRs. However, if you want to customize checks for draft PRs, you can do so under `Override required checks` in the Parallel Mode section.

![Override required checks for draft PRs.](<../../.gitbook/assets/Screen Shot 2022-07-13 at 4.37.18 PM.png>)

### Option 2: Dashboard Yaml Configuration

On the [<mark style="color:blue;">Merge Rules</mark>](https://app.aviator.co/github/rules) page, there is a `Yaml configuration` tab that allows you to both update and validate a configuration file.

![](<../../.gitbook/assets/Screen Shot 2022-05-23 at 5.38.57 PM.png>)

### Option 3: Create Yaml File within your repository

You can also create a configuration file stored in `.mergequeue/config.yml`. The file will only be read once it is merged into the repository's default branch. It will also override any properties set in the Dashboard UI. The only required attribute is `merge_rules`.`labels.trigger.`

<details>

<summary>Complete Yaml Configuration</summary>

```yaml
version: 1.0.0
merge_rules:
  labels:
    trigger: "label_name"
    skip_line: "skip_line"
    merge_failed: "blocked"
    skip_delete_branch: "do-not-delete"
  update_latest: true
  delete_branch: false
  use_rebase: false
  publish_status_check: true
  base_branches:
    - master
    - /release-*/
  enable_comments: true
  ci_timeout_mins: 60
  require_all_checks_pass: false
  preconditions:
    validations:
    - name: missing JIRA ticket in PR title
      match:
        type: title
        regex:
        - -\s\[[^\]]*\]
        - ()
    - name: validation_body
      match:
        type: body
        regex:
        - -\s\[[^\]]*\]
    number_of_approvals: 1
    required_checks:
      - check_1
      - "check 2"
      - name: conditional_check
        acceptable_statuses:
          - skipped
          - success
    use_github_mergeability: true
    conversation_resolution_required: false
  merge_mode:
    type: "parallel"
    parallel_mode:
      use_affected_targets: false
      use_fast_forwarding: false
      max_parallel_builds: 10
      max_requeue_attempts: 3
      stuck_pr_label: "label"
      stuck_pr_timeout_mins: 90
      block_parallel_builds_label: "block_batch"
      check_mergeability_to_queue: false
      override_required_checks:
        - "check 1"
        - check_2
      batch_size: 1
      batch_max_wait_minutes: 0
      require_all_draft_checks_pass: false
  auto_update:
    enabled: false
    label: "auto_update"
    max_runs_for_update: 10
  merge_commit:
    use_title_and_body: true
    cut_body_before: "----"
    cut_body_after: "+++"
  merge_strategy:
    name: "squash"
    override_labels:
      squash: "mq-squash"
      rebase: "mq-rebase"
      merge: "mq-merge"
```

</details>
