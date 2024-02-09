# Enable in a sub-repository

You can start enabling FlexReview features from some parts of your repository.

## Automatically suggest a reviewer

You can run `/aviator flexreview suggest` on any PR to get a FlexReview suggestion. FlexReview can post this suggestion comment automatically when a PR is created.

1. Go to [<mark style="color:blue;">the FlexReview config page</mark>](https://app.aviator.co/repo/aviator-co/mergeit/flexreview/config)
2. Add a directory path that you want to suggest a reviewer automatically in the Reviewer Suggestion config
3. Click the save button

With this config, FlexReview will post a reviewer suggestion comment for a new PR that touches the files under that directory. You can specify `*` here to post a suggestion comment to all new PRs.

## Approval Check

You can require a PR to be reviewed by an expert before merge.

1. Go to [<mark style="color:blue;">the FlexReview config page</mark>](https://app.aviator.co/repo/aviator-co/mergeit/flexreview/config)
2. Add a directory path that you want to suggest a reviewer automatically in the Approval Check config
3. Click the save button

With this config, FlexReview will post [<mark style="color:blue;">a GitHub status check</mark>](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/collaborating-on-repositories-with-code-quality-features/about-status-checks) on a new PR that touches the files under that directory. This status check is looking into the reviewers and their approvals, and it checks if the approvals satisfy the expert approval requirements on the modified files.
