# Slash commands

### Suggest

The suggest command gets a summary of the suggested reviewers for your PR. Additional information such as the author’s expertise score and alternate reviewers is also provided. The results will be posted as a GitHub comment.

If you are using a `CODEOWNERS` file, FlexReview will automatically suggest reviewers from within the `CODEOWNERS` scope. If you want to check the suggested reviewers without the `CODEOWNERS` restriction, you can add the `--no-codeowners` parameter to the command.

```jsx
/aviator flexreview suggest --no-codeowners
```

### Show Status

The show status command will create a GitHub check run that verifies whether the PR has received all required expert approvals.

```jsx
/aviator flexreview show-status
```

The check run will show up in the GitHub checks UI, and the details provide information on pending approvals.

<figure><img src="../../.gitbook/assets/Screen Shot 2024-02-06 at 8.14.39 PM.png" alt=""><figcaption></figcaption></figure>
