# Stacked PRs

Stacked PRs are managed primarily using the [<mark style="color:blue;">`av`</mark> <mark style="color:blue;"></mark><mark style="color:blue;">command line tool</mark>](../aviator-cli/).

## How does it work?

Under the hood, a stack of PRs is like a linked-list of Git branches.

Aviator allows you to create and update these stacked branches while keeping all branches in sync with the branch they are stacked on (with the `av` CLI) and merge stacked PRs into your repository (using MergeQueue).
