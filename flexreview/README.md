# FlexReview

{% hint style="success" %}
Read our launch post about why we built FlexReview: [https://www.aviator.co/blog/flexreview-a-flexible-code-review-framework/](https://www.aviator.co/blog/flexreview-a-flexible-code-review-framework/)
{% endhint %}

## What is FlexReview

An alternative to CODEOWNERS for faster code review cycles.

FlexReview introduces flexibility to your code review process by understanding the nuances of every code change and every reviewer. Instead of defining a static `CODEOWNERS` file, it analyses the history of code review patterns in your repository to calculate an expert score for every file and every developer. It uses this score along with reviewer availability, reviewer workloads, and the complexity of code changes to determine the right reviewers.

{% embed url="https://www.youtube.com/watch?v=jFkKZUSrEvA" %}

## Why FlexReview

As the team grows, reviews can be become slow and unyielding, and occasionally turn into mere rubber-stamping. FlexReview is separating out the concepts of code ownership and review cycles to streamline the development cycle.

### Codeowners today

The codeowners feature allows you to specify individuals or teams who are responsible for certain code paths in a repository. They are automatically assigned as reviewers for any code change that modifies the respective code path.

The `CODEOWNERS` config works in teams where code is well structured and isolated, but it doesn’t scale well for large teams. Here’s why:

* A change to an import statement and 1000 lines of feature work are treated the same
* A small number of reviewers can get overwhelmed with the majority of reviews
* There is no context for domain experience, all reviewers are considered alike
* The `CODEOWNERS` file can be hard to maintain, and becomes too brittle over time
* Reviewers often times only want to track changes vs. take time to properly review the PRs
* Unable to set more complex requirements - for example, there’s no way to set permissions if you want reviews from both the product team and the security team

Code review assignment is also messy:

* Hard to know who to assign reviewers in other teams
* It won't consider out-of-office schedules / timezones of the reviewer / reviewee
* It won't allow assigning primary / secondary reviewers for training purposes
* It won't consider the review load of the reviewer - some people already have too many PRs to review

The key challenge with the codeowners feature is that it tries to define ownership, reviewers, and gatekeeping all in one file.

## How it works

FlexReview works directly from your GitHub interface. It connects with your GitHub repository as the Aviator app and analyzes past code review patterns. Once analysis is complete, it can start suggesting reviewers, validate code approval requirements, and track SLOs. It can either work alongside an existing `CODEOWNERS` file or completely replace it.

FlexReview has four major components:

### Instant reviewer suggestions

When a pull request is created or marked ready for review, FlexReview automatically suggests or assigns the reviewers and offers a detailed breakdown on why these reviewers were suggested. It also offers some alternate reviewers.

### Smart code approval validation

FlexReview reduces required reviewers for low-risk changes and allows for broader participation in code review knowledge sharing. It relaxes the reviewer requirement for PRs that have minor changes and when the authors themselves possess extensive code knowledge.

### **SLO management for review response**

Establish a Service Level Objective (SLO) for code reviews across your organization or at a team level. Track SLOs based on code complexity, business hours, and holidays to optimize your code review feedback cycles. Set separate SLO targets for both inter-team and intra-team reviews while configuring automated actions based on when these targets are breached.

### AttentionSet for PR management

Inspired by [<mark style="color:blue;">Gerrit</mark>](https://gerrit-review.googlesource.com/Documentation/user-attention-set.html), AttentionSet helps you stay on top of your pull requests by tracking PRs awaiting your action, passing attention to others without nagging, and identifying PRs that are getting stale.

## Future work

FlexReview is starting from a low-config mode to remove some of the assumptions for how code reviews are managed. As it evolves, it will naturally introduce some level of configurability and policies to service specialized scenarios. Check out our roadmap for further details.
