# Configuration

FlexReview is a low-config tool. That means you do not need to specify exact ownership for each file or directory. These are calculated dynamically based on historical data.

There are a few configuration properties that help you modify how FlexReview behaves. All the configuration values can be set on the [<mark style="color:blue;">FlexReview Config page</mark>](https://app.aviator.co/flexreview/config).

## Enable / Disable

To enable FlexReview for a specific repository, select that repository from the dropdown and click the **Enable** button on the top right. This will start indexing the past pull request data but keep the repository in [<mark style="color:blue;">read-only mode</mark>](../concepts/read-only-mode.md). To disable it for a specific repository, click the **Disable** button from the FlexReview config page.

The same can also be done from the [<mark style="color:blue;">Repositories list</mark>](https://app.aviator.co/github/repos) page.

## Sub-repo configurations

<figure><img src="../../.gitbook/assets/Screenshot 2024-02-10 at 2.52.00 PM.png" alt=""><figcaption></figcaption></figure>

Sub-repo configuration allows you to activate FlexReview for some parts of your repository while keeping it in read-only mode in other parts. Both of the sub-repo configuration file path uses the [<mark style="color:blue;">gitignore file pattern format</mark>](https://git-scm.com/docs/gitignore#\_pattern\_format).

### Reviewer Suggestion

Specifies which file paths Aviator should [<mark style="color:blue;">suggest reviewers for</mark>](../concepts/reviewer-suggestion-and-assignment.md). Multiple file paths are supported, one on each line. The file paths can be specified in the [<mark style="color:blue;">gitignore file pattern format</mark>](https://git-scm.com/docs/gitignore#\_pattern\_format).

Aviator will publish a suggestion comment for every pull request that touches a file in these whitelisted file paths. The suggestion provided by Aviator will always cover **all** files in the PR, even if the PR only has some files that are covered under the whitelisted file path. Read more in how to [<mark style="color:blue;">get a reviewer suggestion</mark>](../how-to-guides/get-reviewer-suggestion.md).

### Approval Check

Specifies which file paths Aviator should report a GitHub status check for. This status check is used to report the [<mark style="color:blue;">approval requirement</mark>](../concepts/approval-requirements.md) for the PR. Multiple file path are supported, one on each line. The file paths can be specified in the [<mark style="color:blue;">gitignore file pattern format</mark>](https://git-scm.com/docs/gitignore#\_pattern\_format).

Aviator will publish a GitHub status check for every pull requests that touches a file in these whitelisted file paths. The status check provided by Aviator will always cover **all** files in the PR, even if the PR only has some files that are covered under the whitelisted file paths. Read more in how to [<mark style="color:blue;">mandate an expert approval</mark>](../how-to-guides/mandate-an-expert-approval.md).

You can also set the Approval check to use the same file paths as the Reviewer Suggestions. That way, if you modify the Reviewer Suggestions file paths, the Approval Checks file paths will automatically be updated.

## Reviewer Exclusion

In a large organization, it may be desired to exclude certain reviewers from being suggested. These could be SREs, managers, product managers, designers or other inactive collaborators who may have access to the code base, and have contributed in the past but are passive collaborators now.&#x20;

The excluded reviewers are still accepted for approval check validation, but are never suggested by FlexReview.

To exclude the reviewers, enter their GitHub handles on the [<mark style="color:blue;">FlexReview config page</mark>](https://app.aviator.co/flexreview/config). Enter one username per line.

<figure><img src="../../.gitbook/assets/Screenshot 2024-02-10 at 2.43.57 PM (1).png" alt=""><figcaption></figcaption></figure>

## CODEOWNERS

This configuration lets you specify a custom path for the `CODEOWNERS` file. One of the core advantages of using FlexReview is that it reduces the total number of reviewers required for each PR when compared to the default `CODEOWNERS` assignment. Using a non-default path ensures that GitHub will not auto-assign reviewers for a pull request, and lets FlexReview assign reviewers instead.

This configuration is completely optional. If your team does not use a `CODEOWNERS` file, or if you want to keep the default path for `CODEOWNERS`, you can keep this blank.

### Assignment conflict

When auto-assignment is enabled but GitHub is also assigning reviewers based on the `CODEOWNERS` file, Aviator will replace the default reviewers with the suggested ones. Note that depending on your team's configuration, this may still notify the default reviewers before they are removed from the reviewers list.
