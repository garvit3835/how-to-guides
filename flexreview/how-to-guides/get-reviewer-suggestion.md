# Get a reviewer suggestion

### Manual Slash command

With the Aviator app installed on your GitHub repository, it is possible to request suggested reviewers from FlexReview. This is done using a GitHub comment slash command:

```
/aviator flexreview suggest
```

FlexReview will post a comment showing the recommended reviewers, the PR author's expertise score, and alternate reviewers.

### View suggested reviewers without adding comments to a PR

The Aviator webapp provides a way to see FlexReview suggestions without posting a comment to your pull requests. Visit [<mark style="color:blue;">Aviator FlexReview Suggest</mark>](https://app.aviator.co/flexreview/suggest).

Choose the respective repository.

![WebApp Repo Choice](../../.gitbook/assets/top\_of\_revsug\_dock.png)

Then enter a pull request number to see suggested reviewers.

![WebApp Suggested Reviewer](../../.gitbook/assets/FR\_webapp\_example.png)

### Automatic reviewer suggestion on PRs

Automatic reviewer suggestion can be configured in the Aviator web app. See the [<mark style="color:blue;">configuration page.</mark>](https://app.aviator.co/flexreview/config) With this enabled, Aviator will post an automatic FlexReview suggestion on to pull requests that contain files matching the file path prefix.

![WebApp Sub Repo paths](../../.gitbook/assets/FR\_sub-repo-config.png)

## See Also

* [<mark style="color:blue;">Aviator Slash Commands</mark>](../../mergequeue/reference/slash-commands.md) For Aviator slash commands reference.
