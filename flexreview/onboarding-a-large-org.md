# Onboarding a large org

Moving away from the strict Codeowners concept is a big cultural change. This requires carefully planning and testing the FlexReview approach. If you do not use `CODEOWNERS` requirements in your GitHub repository, you can skip this doc.

### Why migrate

Migrating away from the strict controls of the Codeowners concept can bring several benefits to your team. By adopting FlexReview, you can automate the assignment of reviewers based on their expertise, rather than solely relying on ownership of code files. That means:

* No need to maintain a static file declaring ownership
* Ensure that the most qualified individuals provide feedback on each pull request
* Drastically reduce the number of reviewers required per code change
* Improve code review response times significantly

### Starting with one directory

To ease the transition and familiarize your team with FlexReview, we recommend starting with one specific directory in your repository. By selecting a single directory, you can test and evaluate the functionality of FlexReview in a controlled environment before expanding its usage to other parts of your repository. This approach allows you to gradually adapt to the new review process while minimizing any potential disruptions to your existing workflows.

#### Read-only mode

When you install the Aviator app for FlexReview, it starts in [**read-only mode**](concepts/read-only-mode.md), that is, it won’t take any actions or post any comments on the PRs automatically.

At this stage, you can use the slash command on any PR to get a suggestion from FlexReview:

```markdown
/aviator flexreview suggest
```

This will suggest reviewers and post a message with a list of suggested reviewers. This will display the list of files and corresponding need for experts based along with suggested reviewers. If you already use `CODEOWNERS` file, it will suggest reviewers that also satisfy the Codeowners requirement.

None of these commands change the PR merging process and no status check is reported.

#### Sub-directory mode

You can specify one or more code paths that you would like FlexReview to find reviewers for. You would ideally want to choose a team that gets a lot of review requests from outside of that team. That’s because these might be the reviews that hits the most bottlenecks and would benefit the most.

When specified, all PRs that touch these file paths will get a suggested reviewers GitHub comment from FlexReview. Since these directories are managed by Codeowners, FlexReview will only suggest valid reviewers based on the Codeowners file, and provide a status check. You can ignore the status check for now.

This would be a good opportunity to start educating the team about how FlexReview works and how to use it. You can continue using it in `manual` mode where the reviewers are suggested as comments so that the author can decide whether to add them to the list of reviewers.

#### Expanding code ownership by one-level

To get the real benefit of FlexReview, you should edit the `CODEOWNERS` file and expand the ownership of the sub-directory to the wider engineering org. You can start by expanding it by one-level.

<figure><img src="../.gitbook/assets/engineering-teams (2).png" alt=""><figcaption><p>Expanding ownership in engineering teams</p></figcaption></figure>

Let’s say a sub-directory has a team ownership of `ios-auth-eng` that manages the authentication framework for iOS, and there is a GitHub team `ios-eng` and a wider `eng` team. Here, you can replace that ownership of this sub-directory in the codeowners file to: `ios-eng` so that anyone on the ios engineering team can approve this change (from a `CODEOWNERS` perspective).

```markdown
ios/authentication/ @ios-en
```

At this stage, FlexReview will start handling approval requirement, and introducing additional experts in the mix depending on code change complexity. Additionally, FlexReview will automatically start validating the complex reviews with the expert reviewers.

#### Expanding code ownership beyond

Once you have tested this with a few PRs, you can expand this further, and edit the `CODEOWNERS` file to all of `eng` for this sub-directory and continue testing it.

If you have chosen a good sub-directory / project to start with, it’s likely that the developers are now familiar with how FlexReview works and comfortable with the idea of flexible reviews and expert reviewers. You can continue expanding the same way for additional sub-directories or onboarding everyone together.

### Final steps

At this point, reviewers were automatically or manually assigned to each PR, replacing the default reviewers / teams assigned by GitHub. To skip the auto-assignment of GitHub reviewers completely, you can rename the CODEOWNERS file and update the path of that file in the FlexReview configurations.

You can now also make the FlexReview status check required in GitHub branch protection settings.
