# Chrome Extension

You can use [<mark style="color:blue;">Aviator's Chrome Extension</mark>](https://chrome.google.com/webstore/detail/aviator-chrome-extension/inoabloekooadaolcncfmpgafkgbgnif) to enqueue your pull-request from GitHub.

## Installation

Install from [<mark style="color:blue;">Chrome Web Store</mark>](https://chrome.google.com/webstore/detail/aviator-chrome-extension/inoabloekooadaolcncfmpgafkgbgnif). After the installation, open your pull-request, and you can find a log-in button instead of the GitHub merge button.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Click the login button, and you'll redirected to the Aviator login page. If you are already logged in, it should automatically get back to the original PR page.

## How to use

If the repository is configured with Aviator MergeQueue, it shows a button to enqueue a pull request. This will work properly with stacked PRs as well.

<figure><img src="../.gitbook/assets/Screen Shot 2023-11-07 at 3.22.42 PM.png" alt=""><figcaption><p>Enqueue an open PR</p></figcaption></figure>

Once the PR has entered the queue, the extension will show information about the bot pull request, and a timeline of the PR's activity. If the queue is currently paused, the extension will notify the user, regardless of the PR's status.

<figure><img src="../.gitbook/assets/Screen Shot 2023-11-03 at 5.03.32 PM.png" alt=""><figcaption><p>PR in the merge queue</p></figcaption></figure>

The extension will show updated information if the PR is blocked.

<figure><img src="../.gitbook/assets/Screen Shot 2023-11-07 at 3.31.41 PM.png" alt=""><figcaption><p>Blocked PR</p></figcaption></figure>

If you need, there is an option to show the original GitHub merge button.

## Use with Aviator MergeQueue on-prem

You can open the extension option page from the extension menu.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

You can specify your on-prem Aviator deployment URL.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
