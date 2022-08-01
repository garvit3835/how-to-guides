# Stacked PRs

Stacked PRs are managed primarily using the [`av` command line tool](../aviator-cli/).

## How does it work?

Under the hood, a stack of PRs is like a linked-list of Git branches.

Aviator allows you to create and update these stacked branches while keeping all branches in sync with the branch they are stacked on (with the `av` CLI) and merge stacked PRs into your repository (using MergeQueue).
