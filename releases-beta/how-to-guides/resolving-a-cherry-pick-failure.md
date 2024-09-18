# Resolving a Cherry-Pick Failure

This guide walks you through the steps to handle a failed cherry-pick during a release process in Aviator. Cherry-picking allows you to selectively integrate specific changes (commits) into a base release. If a conflict occurs, you can resolve it manually.

Setup a repository, connect it to Aviator and [create a release project](./creating-a-release-project.md) for this guide.

## Create a Base Release
1. Add a new file to your repository. For example, `echo "base" > test.txt`
2. Commit and push the changes to a different branch.
3. Create a pull request to the main branch.
4. Merge the PR into the main branch.
![](../../.gitbook/assets/release-conflict-base.png)
5. Cut a release: Create a new release based on this PR.
![](../../.gitbook/assets/release-conflict-base-cut.png)

## Raise Two more Pull Requests
1. First PR: `Ensure test.txt` contains "base".
2. Modify the file: `echo "modify1" > conflict-test.txt`.
3. Commit and merge the PR.
![](../../.gitbook/assets/release-conflict-modify1.png)
4. Second PR: Confirm that conflict-test.txt shows modify1.
5. Modify the file again: `echo "modify2" > conflict-test.txt`.
6. Commit and merge the PR.
![](../../.gitbook/assets/release-conflict-modify2.png)

### Merge Conflict
Let us create a merge conflict just for the tutorial. Go back to the Aviator Release Dashboard and Select the last PR (the one with modify2) to cherry-pick into the base release.
![](../../.gitbook/assets/release-conflict-cherry-pick.png)

You will see a red tag attached to the release "Failed to cherry-pick" because of the merge conflict. Now if you face a similar issue, you can resolve this by following the below steps.


## Resolve Merge Conflicts
1. On this failed cherry-pick commit, you should see the conflict resolution PR link. Open the link and follow the instructions to resolve the conflict.
![](../../.gitbook/assets/release-conflict-action.png)
2. follow the steps provided by Aviator-app bot to resolve the conflict, ensure you pick modify2 as the final result.
![](../../.gitbook/assets/release-conflict-resolve.png)
3. After pushing the changes to this branch, you should be able to merge the PR successfully. You may optionally also get this reviewed by a coworker for correctness.
![](../../.gitbook/assets/release-conflict-resolve-merge.png)
4. Once you've resolved the conflict and merged the PR, the Release Candidate (RC) should show the correct version of the file with modify2. The release should be created successfully, reflecting the conflict resolution.
![](../../.gitbook/assets/release-conflict-resolve-ui.png)