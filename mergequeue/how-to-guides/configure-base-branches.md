# Configure base branches

You can explicitly specify [base branches ](../concepts/base-branches.md)that Aviator should monitor, this way Aviator will ignore any PRs that target a different base branch. A base branch can also be specified as a glob pattern. Here’s an example config:

```
merge_rules:
  base_branches:
  - master
  - /release-*/
```
