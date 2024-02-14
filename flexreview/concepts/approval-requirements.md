# Approval Requirements

One of the most significant advantages of using FlexReview is the ability to expand the pool of valid code reviewers depending the code complexity. This ensures that more complex PRs are validated by the relevant experts, while the simpler changes can pass through with lighter review requirements. To enforce this custom validation, FlexReview can report a GitHub status check.

<figure><img src="../../.gitbook/assets/Screen Shot 2023-11-14 at 2.59.54 PM (1).png" alt=""><figcaption></figcaption></figure>

Read the [<mark style="color:blue;">mandating an expert approval</mark>](../how-to-guides/mandate-an-expert-approval.md) to learn how to customize the status check when using sub-repository configuration.

## Understanding approval requirements

Like the code reviewer suggestions, the approval requirements also have similar goals:

* Reduce the total number of reviewers needed for each code review, and therefore, reduce iteration time.
* Knowledge sharing - expands the suggested pool of reviewers when applicable.
* Ensure that the `CODEOWNERS` requirement is satisfied, if a code owners file exist.

Under the hood, both the reviewer suggestion and code approval requirements use the same algorithm. The reviewer suggestion service takes the pool of all valid approvers and then suggests reviews based on some additional parameters like workload and availability. That means, the suggested reviewers collectively will always satisfy the approval requirements.

### Code complexity and domain knowledge

Code complexity and domain knowledge of the author are the primary factors that determine the approval requirement. As the code complexity increases, the requirement for an expert review becomes critical. On the flip side, if the author of the PR itself has a strong understanding of the code change, then FlexReview relaxes the expert requirement.

<figure><img src="../../.gitbook/assets/code-approval (1).png" alt=""><figcaption></figcaption></figure>

Note that the code complexity is calculated for each file in the code review, so an expert approval may be needed for some parts of the PR but other parts can be approved by anyone.

The status check Details view will also show the breakdown of which files are pending review and who can review them.

<figure><img src="../../.gitbook/assets/Screenshot 2024-02-05 at 1.44.21 PM.png" alt=""><figcaption></figcaption></figure>

## Example

To understand how validation works, let’s take a look at an example. If a PR has 4 files: `A` , `B` , `C` , `D` that has four different owners / experts `Ea` , `Eb` , `Ec` and `Ed` , then FlexReview may deem files `A` and `B` as files with minor changes. In this case, the FlexReview approval service requires approval from experts in `Ec` and `Ed` but will accept anyone to approve the files `A` and `B` .
