# FlexReview

## What is FlexReview

An alternate to CODEOWNERS for faster code review cycles.

FlexReview introduces flexibility in your code review process by understanding that every code change is different, and every reviewer is different. Instead of defining fine-grained static `CODEOWNERS` file, it analyses past data of code review patterns to calculate an expert score for every file and every developer. It uses this score along with reviewer availability, workloads and the complexity of code change to determine the right reviewers.

## Why FlexReview

As the teams grow reviews can be become slow, unyielding and occasionally turn into a mere rubber-stamping. FlexReview is separating out the concepts of code ownership and code reviewership to improve code review cycle times.

### Codeowners today

The codeowners feature allows you to specify individuals or teams who are responsible for certain code paths in a repository. This way, they are automatically assigned as reviewers for any code change that modifies the respective code path.

Although the `CODEOWNERS` config works in teams where code is well structured and isolated, it doesn’t scale well for large teams. Here’s why:

* Whether it’s a change in import statement, or a 1000 lines feature work, the rules are the same
* Small number of reviewers can get overwhelmed with most of the reviews
* There’s no context of domain experience, all reviewers are considered alike
* The `CODEOWNERS` file can be hard to maintain, becomes too brittle over time
* Many times reviewers only want to track changes vs take time to properly review the PRs
* No way to set more complex requirements, for example, there’s no way to set permissions if you want review from both product team and security team.

But also code review assignment is messy:

* Hard to know who to assign reviewers in other teams
* It won't consider out-of-office schedules / timezones of the reviewer / reviewee.
* It won't allow assigning primary / secondary reviewers for the training purpose.
* It won't consider the review load of the reviewer. Some people already have too many PRs to review.

The key challenge with codeowners feature is that it tries to define ownership, reviewers and gatekeeping all in one file.

## How it works

FlexReview works directly from your GitHub interface. It connects with your GitHub repository as the Aviator app and analyzes the past code review patterns. Then it can start suggesting reviewers, validate code approval requirements and track SLOs. It can either work alongside an existing Codeowners file or completely replace it.

FlexReview has four major components:

### Instant reviewer suggestions

When a Pull Request is created or marked ready for review, FlexReview automatically suggests or assigns the reviewers and offers a detailed breakdown on why these reviewers were suggested. It also offers some alternate reviewers

### Smart code approval validation

FlexReview reduces required reviewers for low-risk changes and allow broader participation in the code review fostering knowledge sharing. It relaxes the reviewer requirement for PRs that have minor changes and where the author themselves possess extensive code knowledge.

### **SLO management for review response**

Establish a Service Level Objective (SLO) for code reviews across your organization or at a team level. Track SLOs based on code complexity, business hours, and holidays to optimize your code review feedback cycles. Set separate SLO targets for both inter-team and intra-team reviews while configuring automated actions based when these targets are breached.

### AttentionSet for PR management

Inspired by [<mark style="color:blue;">Gerrit</mark>](https://gerrit-review.googlesource.com/Documentation/user-attention-set.html), AttentionSet helps you stay on top of your pull requests by tracking PRs awaiting your action, passing attention to others without nagging, and identifying PRs that are getting stale.

## Future work

FlexReview is starting from a low-config mode to remove some of the presumptions for how code reviews are managed. As it evolves, it will naturally introduce some level of configurability and policies to service specialized scenarios. Check out our roadmap for further details.
