# Onboarding a large org

Moving away from strict controls of Codeowners concept is a big cultural change. This would require carefully planning and testing the FlexReview approach. If you do not use `CODEOWNERS` requirements in your GitHub repository, you can skip this doc.

### Why migrate

Migrating away from the strict controls of the Codeowners concept can bring several benefits to your team. By adopting FlexReview, you can automate the assignment of reviewers based on their expertise, rather than solely relying on ownership of code files. That means:

* No more need to maintain a static file wit the ownership,
* ensure that the most qualified individuals provide feedback on each pull request,
* drastically reduce the number of reviewers required per code change,
* improve the code review response times significantly

### Starting with one directory

To ease the transition and familiarize your team with FlexReview, we recommend starting with one specific directory in your repository. By selecting a single directory, you can test and evaluate the functionality of FlexReview in a controlled environment before expanding its usage to other parts of your repository. This approach allows you to gradually adapt to the new review process while minimizing any potential disruptions to your existing workflows.

#### Read-only mode

When you install the Aviator app for FlexReview, it starts in the **read-only mode**, that is, it won’t take any actions or post any comments on the PRs automatically.

At this stage, you can use slash command on any PR to get the suggestion from FlexReview:

```markdown
/aviator flexreview suggest
```

This will suggest reviewers and post a message giving list of suggested reviewers. This will display the list of files and corresponding need for experts based along with suggested reviewers.

These suggestions would still take into consideration the existing `CODEOWNERS` file and who can officially approve the PR based on that. To test the suggestions without gating it on codeowners, add a param: `--no-codeowners`:

```markdown
/aviator flexreview suggest --no-codeowners
```

This will suggest reviewers assuming that there is no codeowners file.

None of these commands change the PR merging capability and no status check is reported.

#### Sub-directory mode

At this time, you can specify one or more code paths that you would like FlexReview to find reviewers for. You would ideally want to choose a team that gets a lot of review requests from outside of that team. That’s because these might be the reviews that gets bottleneck’ed most often, and would see most benefit.

When specified, all PRs that touch these file paths will get suggested reviewers GitHub comment from FlexReview. Since these directories are managed by Codeowners, FlexReview will only suggest valid reviewers based on the Codeowners, and provide a status check for the same. You can continue to ignore the status check for now

This would be a good opportunity to start educating the team about how FlexReview work and how to work with it. You can continue using it in `manual` mode where the reviewers are suggested as comments so that the author can decide and add them to the list of reviewers.

#### Expanding code ownership by one-level

To get the real benefit of FlexReview, you should edit the `CODEOWNERS` file and expand the ownership of the sub-directory to the wider engineering org. You can start by expanding it to one-level.

<figure><img src="../.gitbook/assets/engineering-teams (2).png" alt=""><figcaption><p>Expanding ownership in engineering teams</p></figcaption></figure>

Let’s say a sub-directory has a team ownership of `ios-auth-eng` that manages the authentication framework for iOS, and there is a GitHub team `ios-eng` and a wider `eng` team. Here, you can replace that ownership of this sub-directory in the codeowners file to: `ios-eng` so that anyone in ios engineering team can approve this change (from codeowners perspective).

```markdown
ios/authentication/ @ios-en
```

At this stage, FlexReview will start handling approval requirement, and introducing additional experts in the mix depending on code change complexity. Additionally FlexReview will start validating the complex reviews with the expert reviewers automatically.

#### Expanding code ownership beyond

Once you have tested this with a few PRs, you can expand this further, and edit the codeowners file to all of `eng` for this sub-directory and continue testing it out.

If you have chosen a good sub-directory / project to start with, it’s likely that the developers are now familiar with how FlexReview works and get comfortable with the idea of flexible reviews and expert reviewers. Now, you can continue expanding the same way for additional sub-directories or onboarding everyone together.

### Final steps

So far the assignment meant the reviewers were automatically or manually assigned to each PR, replacing the default reviewers / teams assigned by GitHub. To skip the auto-assignment of GitHub reviewers completely, you can rename the CODEOWNERS file and update the path of that file in the FlexReview configurations.

You can now also make the FlexReview status check as required in GitHub branch protection settings.
