# Stacked PRs

{% embed url="https://youtu.be/s67j48mJ4_s" %}
Demo video using the `av` CLI and MergeQueue to managed stacked PRs
{% endembed %}

## Motivation

Stacked PRs are useful when you have a lot of code changes that are hard to review in a single PR. Stacked PRs let you break down the changes into smaller PRs that are “stacked” on top of each other, keeping code review manageable. This means you can keep working on the next bit of code related to a feature even while waiting for your previous PR(s) to pass CI, be approved, and merged!

For example, if PR1 is `Add the /books/list route to the backend` and PR2 is `Show the books list in the frontend`, we can stack PR2 on top of PR1. This means that you don’t have to wait for PR1 to be reviewed and merged before you can start PR2 (and they can even be reviewed by separate people!).

## Stacked PRs Quickstart

Aviator helps you manage stacked PRs at two points during the code lifecycle: during authoring (with the `av` CLI) and during the merge process (using MergeQueue).

### Installing av

Follow the [<mark style="color:blue;">Aviator CLI installation instructions</mark>](../reference/aviator-cli/installation.md) to download and install the `av` tool. Make sure to add a GitHub access token to your settings file and run `av init` in the repository where you'll be working with stacked PRs.

### Creating a stack

A stack is (conceptually) a linked list of branches, so the way we start a stack is by creating a normal Git branch!

Creating the first branch in the stack is just the same as creating any normal branch in your repository.

```bash
# Create a new branch off of your repository default branch
# (usually main or master)
av stack branch "bookstore-backend"

# Now we can do our development work as normal.
mkdir ./books
echo '...' >> ./books/backend.py
git add ./books
git commit -m "Add /books/list route to backend"
```

When it comes time to create a PR, you can do so via the GitHub UI or using the `av` CLI.

```shell
av pr create
```

To create the second branch in the stack, we need to use the `av stack branch` command. It requires one argument which is the name of the branch to create. The `av stack branch` command creates a "normal" Git branch (though we're branching from `bookstore-backend` instead of `main` since we want to build off of our previous work) and also sets some internal data to be able to recognize that `bookstore-frontend` is dependent on `bookstore-backend`.

```bash
av stack branch "bookstore-frontend"
```

Again, we can do our feature development work as we would normally.

```bash
echo '...' >> ./books/frontend.html
git add ./books
git commit -m "Add books page to frontend"
```

And when it comes time to submit our work as PR, we use the `av pr create` command again.

```bash
av pr create
```

When creating this PR, we set the base branch in GitHub as `bookstore-backend` rather than `main` to ensure that GitHub shows the diff between `bookstore-frontend` and `bookstore-backend` (otherwise, it would show all the changes from `bookstore-backend` in the PR for `bookstore-frontend` which would make code review much harder).

### Merging a stack

Merging a stack with Aviator is a little different than merging a non-stacked PR.

{% hint style="warning" %}
Make sure not to use the "Merge" button in the GitHub UI for a stacked PR, as this will usually not do what you expect and can be hard to undo. Always use Aviator to merge a stacked PR. [<mark style="color:blue;">See FAQs for more information.</mark>](../reference/stacked-prs/faqs-and-troubleshooting.md#what-happens-if-i-use-the-github-merge-button-instead-of-aviator-to-merge-a-stacked-pr)<mark style="color:blue;"></mark>
{% endhint %}

When merging a stack, we usually want to merge the whole stack (or a subset of it that we've decided is ready for merge) rather than individual PRs one-by-one. When you queue a stacked PR for merge, we merge that PR _and everything that comes before it_ all in one go. To make it obvious that this behavior is happening, we require developers to issue the `/aviator stack merge` command on the GitHub PR (as a [<mark style="color:blue;">slash command comment</mark>](../reference/slash-commands.md)).

{% hint style="info" %}
The `/aviator stack merge` command should be given in the form of a comment on the GitHub pull request page in your web browser. Merging/queueing a PR via the command line is not yet supported.
{% endhint %}

If you merge a sub-stack, you'll have to rebase the subsequent PRs in the stack after merging using `av stack sync`.

### Updating a stack

Since stacked PRs are designed to make code review easier and more incremental, it's likely that you'll need to change code that you wrote earlier in the stack. The `av stack sync` command is used to make sure every branch is up-to-date with its stack parent.

To edit a branch that is part of a stack, first we need to check it out.

```
git checkout bookstore-backend
```

Then, we can make edits and commit as usual.

```bash
echo '...' >> ./books/backend.py
git add ./books
git commit -m "Fix 500 error on malformed input"
```

Finally, we can run `av stack sync` to propagate the changes to all children branches.

```bash
$ av stack sync
```
