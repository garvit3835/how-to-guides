# Mandate an expert approval

In order to mandate expert approvals on PRs, you can use FlexReview’s Approval Check along with required status checks.

In the FlexReview configuration, set up the file paths under `Approval Check`. You can use the same file paths as the reviewer suggestion file paths, or specify something else. This will add an `aviator/flexreview` GitHub status check.

<figure><img src="../../.gitbook/assets/Screenshot 2024-02-10 at 3.30.28 PM.png" alt=""><figcaption><p>Details of the GitHub status check</p></figcaption></figure>

#### Set up MergeQueue required status checks

If your repo is set up to use Aviator’s MergeQueue, navigate to the MergeQueue config. Make sure to set `aviator/flexreview` as a required check. This requires the PR to have an expert approval before Aviator will merge the PR.

#### Set up GitHub required status checks

If your repo is not set up to use MergeQueue, you can simply add `aviator/flexreview` as a required check on GitHub. Read more about GitHub required status checks [<mark style="color:blue;">here</mark>](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches#require-status-checks-before-merging).
