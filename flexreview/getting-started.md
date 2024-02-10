# Getting started

FlexReview is a low-config tool that uses the past pull requests information for dynamically understanding the code ownership.

### Step 1: Connect

During the initial setup, you will be asked to connect Aviator GitHub app  to your GitHub repository to use FlexReview.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-02-09 at 12.08.18 PM.png" alt=""><figcaption></figcaption></figure>

### Step 2: Activate

At this point, you can choose the repository you want to start with. When activating FlexReview on a GitHub repository, it'll be enabled in the [<mark style="color:blue;">**read-only mode**</mark>](concepts/read-only-mode.md). That means, FlexReview won't interact with any PRs directly, but you will be able to use the Slash commands. This gives you an opportunity to fine tune and test out FlexReview before turning on the automation,

<figure><img src="../.gitbook/assets/Screenshot 2024-02-09 at 12.11.36 PM.png" alt=""><figcaption></figcaption></figure>

### Step 3: Test suggestions

FlexReview make take a few seconds to index some teams and pull requests data from your repository at this stage. Once indexed, you should be able to test out the suggestions. Remember that the suggestions here are still imperfect since FlexReview has done a shallow indexing. The full indexing happens in the background.\


Just enter the pull request number in the textbox to see the suggestions on this page, or use the Slash command in GitHub comments to get suggestions directly in your pull request view.

<figure><img src="../.gitbook/assets/flexreview-comment.png" alt=""><figcaption><p>Slash command to get suggestions in the GitHub PR details page</p></figcaption></figure>

{% hint style="info" %}
If it takes longer than 60 seconds to index the initial data, please try refreshing the page, or contact us at **howto@aviator.co**
{% endhint %}

### Step 4 - Config

FlexReview requires minimal config. Once you have tested the usage of Aviator FlexReview manually using Slash commands, you can turn on the auto suggestions and the approval checks on the [<mark style="color:blue;">config page</mark>](https://app.aviator.co/flexreview/config). To simply activate this for the entire repository, just add `*` in the Reviewer Suggestion and Approval Check settings. All subsequent pull requests will then start seeing the suggested reviewers and GitHub status checks when they are ready for review.



<figure><img src="../.gitbook/assets/Screenshot 2024-02-09 at 12.32.18 PM.png" alt=""><figcaption></figcaption></figure>

To learn more about how FlexReview works, please read the Concepts and How-to-guides and check out the complete reference guide to understand all the configurations.

If you have any questions or comments, please reach out to **howto@aviator.co**
