# Merge Rules

Merge rules are at the core of how the Aviator bot behaves. MergeQueue primarily relies on GitHub Labels to communicate with pull requests. There are several attributes that you can customize for each repository using a yaml configuration file.

### Option 1: Dashboard UI

The best option for a quick setup is via the [<mark style="color:blue;">Merge Rules</mark>](https://app.aviator.co/github/rules) page. However, the UI does not expose all possible customizations.

{% hint style="warning" %}
Some of the Rules listed in the yaml configuration below are not supported in the Dashboard UI. Your repo will require using one of the configuration file options if you want to use advanced rules. We only recommend using the Dashboard UI option if you require a simple setup.
{% endhint %}

The only required setting is the `Label for trigger` -  this will default to `mergequeue`. Once you add this to a PR, Aviator will queue and merge the PR.

![Label for trigger is the only required setting.](<../.gitbook/assets/Screen Shot 2022-05-23 at 5.43.35 PM.png>)

All of the settings in the UI are covered in the [<mark style="color:blue;">Rules</mark>](merge-rules.md#rules) section below. Each setting also has a tooltip that provides more information.

#### Configuring Status Checks

See `Required Checks` under `Merge Preconditions` to set status checks for your PRs that need to be validated before merging. Selecting `Use Github mergeability check` will use all Github required tests for the repo.

![Select Required status checks.](<../.gitbook/assets/Screen Shot 2022-07-13 at 4.34.12 PM.png>)

If your team is using Parallel mode, by default, Aviator bot will use the same checks for original PRs and draft PRs. However, if you want to customize checks for draft PRs, you can do so under `Override required checks` in the Parallel Mode section.

![Override required checks for draft PRs.](<../.gitbook/assets/Screen Shot 2022-07-13 at 4.37.18 PM.png>)

### Option 2: Dashboard Yaml Configuration

On the [<mark style="color:blue;">Merge Rules</mark>](https://app.aviator.co/github/rules) page, there is a `Yaml configuration` tab that allows you to both update and validate a configuration file.

![](<../.gitbook/assets/Screen Shot 2022-05-23 at 5.38.57 PM.png>)

### Option 3: Create Yaml File within your repository

You can also create a configuration file stored in `.mergequeue/config.yml`. The file will only be read once it is merged into the repository's default branch. It will also override any properties set in the Dashboard UI. The only required attribute is `merge_rules`.`labels.trigger.`

<details>

<summary>Complete Yaml configuration</summary>

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

## Rules

### Labels

{% tabs %}
{% tab title="Attributes" %}
| Name                     | Type   | Description                                                                                                                                                                                                           |
| ------------------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **trigger**              | String | (_Required_). This label is used to identify that a pull request is ready to be processed by Aviator bot. Once labeled, the bot will verify that the PR has passed all the required conditions and then merge the PR. |
| **skip\_line**           | String | When tagged with this label, Aviator bot will move the PR to the front of the queue.                                                                                                                                  |
| **merge\_failed**        | String | If the pull request fails to merge, Aviator bot will add this label.                                                                                                                                                  |
| **skip\_delete\_branch** | String | If `delete_branch` is enabled, Aviator bot will skip deleting a branch with this label after merging.                                                                                                                 |
{% endtab %}

{% tab title="Example" %}
```yaml
merge_rules:  
  labels:    
    trigger: "label_name"    
    skip_line: "skip_line"    
    merge_failed: "blocked"    
    skip_delete_branch: "do-not-delete"
```
{% endtab %}
{% endtabs %}

### Preconditions

{% tabs %}
{% tab title="Attributes" %}
| Name                                                                    | Type                                    | Description                                                                                                                                                                                        |
| ----------------------------------------------------------------------- | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **number\_of\_approvals**                                               | Integer                                 | Minimum number of reviewers that should approve the PullRequest before it can qualify to be merged. Defaults to 1.                                                                                 |
| **required\_checks**                                                    | List\[Union\[String, ConditionalCheck]] | Checks that need to pass before Aviator bot will merge the PR. Also supports other acceptable CI statuses if your repo uses conditional status checks. See the Example tab.                        |
| <p><strong></strong></p><p><strong>use_github_mergeability</strong></p> | Boolean                                 | Determines whether to use the default required checks specified in branch protection rules on GitHub. When this setting is enabled, Aviator bot will ignore `required_checks`. Defaults to `true`. |
| **conversation\_resolution\_required**                                  | Boolean                                 | Determines whether Aviator bot will queue the PR only after all conversations are resolved.                                                                                                        |
| **validations**                                                         | List\[Validation]                       | Custom validation rules using regexes for the PR body or title. See the Example tab for more details.                                                                                              |
{% endtab %}

{% tab title="Example" %}
```yaml
merge_rules:  
  labels:    
    trigger: "label_name"  
  preconditions:    
    number_of_approvals: 1    
    required_checks:
    # Note: both a string or a more detailed conditional check is accepted
    # for conditional_check, both skipped and success are considered passing      
      - check_1      
      - name: conditional_check
        acceptable_statuses:
          - skipped
          - success   
    use_github_mergeability: true    
    conversation_resolution_required: false
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
```
{% endtab %}
{% endtabs %}

### Merge Modes

| Name     | Type   | Description                                                    |
| -------- | ------ | -------------------------------------------------------------- |
| **type** | String | Determines the mode. Options are: default, parallel, no-queue. |

#### Parallel Mode

The following are only applicable if the above `merge_mode` is set to `parallel`. You can learn more about [<mark style="color:blue;">Parallel Mode here</mark>](../how-to-guides/parallel-mode/).

{% tabs %}
{% tab title="Attributes" %}
| Name                               | Type          | Description                                                                                                                                                                                            |
| ---------------------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **max\_parallel\_builds**          | Integer       | The maximum number of builds that Aviator bot will run at any time. Defaults to no limit.                                                                                                              |
| **max\_requeue\_attempts**         | Integer       | The maximum number of times Aviator bot will requeue a CI run after failing. Note that PRs will only be requeued if the original PR CI is passing but the draft PR CI fails. Defaults to no requeuing. |
| **stuck\_pr\_label**               | String        | The label that Aviator bot will add if it determines a PR to be stuck.                                                                                                                                 |
| **stuck\_pr\_timeout\_mins**       | Integer       | Aviator bot will determine the PR to be stuck after the specified timeout and dequeue it. Defaults to no time out.                                                                                     |
| **block\_parallel\_builds\_label** | String        | Once added to a PR, no further Draft PRs will be built on top of it until that PR is merged or dequeued.                                                                                               |
| **check\_mergeability\_to\_queue** | Boolean       | If enabled, Aviator bot will only queue the PR if it passes all mergeability checks. Defaults to `false`.                                                                                              |
| **use\_affected\_targets**         | Boolean       | If enabled, allows using [affected targets](../how-to-guides/affected-targets.md) in your repo.                                                                                                        |
| **use\_fast\_forwarding**          | Boolean       | If enabled, uses [fast forwarding](../how-to-guides/fast-forwarding.md) to merge PRs.                                                                                                                  |
| **override\_required\_checks**     | List\[String] | Use this attribute if you would like different checks for your original PRs and draft PRs. The checks defined here will be used for the draft PRs created in Parallel mode.                            |
| **batch\_size**                    | Integer       | The number of queued PRs batched together for a draft PR CI run. Defaults to 1.                                                                                                                        |
| **batch\_max\_wait\_minutes**      | Integer       | The time to wait before creating the next batch of PRs if there are not enough queued PRs to create a full batch. Defaults to 0.                                                                       |
{% endtab %}

{% tab title="Example" %}
```yaml
merge_rules:  
  labels:    
    trigger: "label_name"  
  merge_mode:    
    type: "parallel" # other modes: "default", "no-queue"    
    parallel_mode:      
      use_affected_targets: false
      use_fast_forwarding: true      
      max_parallel_builds: 10      
      max_requeue_attempts: 3      
      stuck_pr_label: "label"      
      stuck_pr_timeout_mins: 90      
      block_parallel_builds_label: "block_batch"      
      check_mergeability_to_queue: false
      override_required_checks:
        - "check 1"
        - check_2
```
{% endtab %}
{% endtabs %}

### Auto Update

{% tabs %}
{% tab title="Attributes" %}
| Name                       | Type    | Description                                                                                                                                                                                                                           |
| -------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **enabled**                | Boolean | If enabled, Aviator bot will keep your branches up to date with the main branch. Defaults to `false`.                                                                                                                                 |
| **label**                  | String  | Aviator bot will only keep branches with this label up to date with the main branch. Leave empty to auto merge without a label. If no label is provided and `auto_update` is enabled, by default the Aviator bot will update all PRs. |
| **max\_runs\_for\_update** | Integer | The maximum number of times Aviator bot will update your branch. Defaults to no limit.                                                                                                                                                |
{% endtab %}

{% tab title="Example" %}
```yaml
merge_rules:  
  labels:    
    trigger: "label_name"  
  auto_update:    
    enabled: true    
    label: "auto_update"    
    max_runs_for_update: 10
```
{% endtab %}
{% endtabs %}

### Merge Commit

{% tabs %}
{% tab title="Attributes" %}
| Name                      | Type    | Description                                                                                                                                  |
| ------------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **use\_title\_and\_body** | Boolean | Determines whether Aviator bot will replace default commit messages offered by Github. Defaults to `false`.                                  |
| **cut\_body\_before**     | String  | A marker string to cut the PR body description. The commit message will contain the PR body after this marker. Leave empty for no cropping.  |
| **cut\_body\_after**      | String  | A marker string to cut the PR body description. The commit message will contain the PR body before this marker. Leave empty for no cropping. |
{% endtab %}

{% tab title="Example" %}
```yaml
merge_rules:
  labels:
    trigger: "label_name"
  merge_commit:
    use_title_and_body: true
    cut_body_before: "----"
    cut_body_after: "+++"
```
{% endtab %}
{% endtabs %}

### Merge Strategy

| Name     | Type   | Description                                                                                                                                                                                                                                                                        |
| -------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **name** | String | Defines the merge strategy to use, the options are "squash", "merge", and "rebase". See the [Github docs](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/about-merge-methods-on-github) for more info. |

#### Override Labels

You can specify overrides to the above merge strategy using Github Labels. When a PR is flagged with these labels along with the trigger label, Aviator bot will merge the PR using this strategy instead of the default strategy. Leave empty for no overrides.

{% tabs %}
{% tab title="Attributes" %}
| Name       | Type   | Description                                                            |
| ---------- | ------ | ---------------------------------------------------------------------- |
| **squash** | String | If marked with this label, the PR will be squashed and merged.         |
| **merge**  | String | If marked with this label, the PR will be merged using a merge commit. |
| **rebase** | String | If marked with this label, the PR will be rebased and merged.          |
{% endtab %}

{% tab title="Example" %}
```yaml
merge_rules:
  labels:
    trigger: "label_name"
  merge_strategy:
    name: "squash"
    override_labels:
      squash: "mq-squash"
      merge: "mq-merge-commit"
      rebase: "mq-rebase"
```
{% endtab %}
{% endtabs %}

### Other

{% tabs %}
{% tab title="Attributes" %}
| Name                       | Type          | Description                                                                                                                                                                                                                                |
| -------------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **update\_latest**         | Boolean       | Determines whether Aviator bot will merge the latest base branch into the current branch of your PR before verifying the CI statuses. Defaults to `true`.                                                                                  |
| **delete\_branch**         | Boolean       | Determines whether Aviator bot will delete the branch after merging. Defaults to `false`.                                                                                                                                                  |
| **use\_rebase**            | Boolean       | Determines if Aviator bot will use rebase to update PRs. This feature is only available in the Pro plan. Please note that you also need to enable `update_latest` for rebase to work. Defaults to `false`.                                 |
| **publish\_status\_check** | Boolean       | Determines if Aviator bot will publish a status check back on the PR. This status check represents the current state of PR in the Aviator queue.                                                                                           |
| **enable\_comments**       | Boolean       | Determines if Aviator bot can add comments on the PR to describe the actions performed on the PR by the bot. Aviator bot comments include information such as failure reasons and the position of the PR in the queue. Defaults to `true`. |
| **ci\_timeout\_mins**      | Integer       | The time before we determine that the CI has timed out. Defaults to no time out.                                                                                                                                                           |
| **base\_branches**         | List\[String] | These branches are the ones that Aviator will monitor as valid branches to merge into. Defaults to your repository default branch as configured on GitHub. Regexes are allowed.                                                            |
{% endtab %}

{% tab title="Example" %}
```yaml
merge_rules:
  labels:
    trigger: "label_name"
  update_latest: true
  delete_branch: false
  use_rebase: false
  enable_comments: true
  ci_timeout_mins: 60
  base_branches:
    - master
    - /release-*/
```
{% endtab %}
{% endtabs %}
