# Reviewer suggestion and assignment

## Goals

The ultimate goal with reviewer suggestion is to reduce the code review response time. To achieve this goal, there are a few additional goals defined by FlexReview:

* Reduce the total number of reviewers needed for each code review - having fewer reviewers helps reduce iteration time
* Knowledge sharing - expands the suggested pool of reviewers wherever applicable
* Ensuring code owners requirement is satisfied, if a code owners file exist

## Analyzing past data

When you enable FlexReview on a repository, it starts in read-only mode and indexes the past pull requests data to calculate the domain expertise score of each author and reviewer for various code paths. It also captures the teams memberships of the developers, and reads the existing `CODEOWNERS` file.

## Calculating the suggestions

For detailed understanding of the calculations, please refer to the calculations algorithm. FlexReview takes into account a few factors to identify the right reviewers:

* complexity of the code change - the complexity is calculated separately for each path in the given PR
* domain expertise score - this is calculated based on the past data of the code reviews. A user can gain domain expertise both by authoring the code as as well reviewing it. Read the algorithm for detailed explanation of this calculation. Domain expertise score is calculated both for the author and the reviewer.
* current review load of all valid reviewer candidates
* availability (timezone, OOO)
* the ownership defined in the codeowners file - the suggester service will choose the least amount of reviewers possible for each code change that satisfy the codeowners requirement if the file exists.

Based on these heuristics, the reviewers are suggested for a pull request. For instance:

* when the code complexity is high and the domain expertise of the author is low, a reviewer with high domain expertise will be chosen
* when the code complexity is low and the domain expertise of the author is high, the pool of reviewers is increased so that non-experts may also be chosen based on the current review load, ownership and availability

## Reviewer Assignment

Using the suggestions, the reviewer assignment can be manual or automatic. In case of manual reviewer assignment, a comment is posted with suggested reviewers and a detailed breakdown analysis of the suggestion.

<figure><img src="../../.gitbook/assets/flexreview-comment (1).png" alt=""><figcaption></figcaption></figure>

In case of automatic reviewer assignment, the suggested reviewers are automatically assigned ot the PR at the time of suggestion. If GitHub assigns reviewers based on Codeowners, FlexReview would replace the assigned reviewers based on the suggested reviewers.

Note that, if you are using the default `CODEOWNERS` file path, then GitHub will automatically assign teams or specific reviewers. In this case, FlexReview can override the add or remove reviewers.
