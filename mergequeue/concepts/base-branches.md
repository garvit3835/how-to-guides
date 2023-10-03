# Base Branches

By default, Aviator monitors all PRs in a repository and will enqueue all of them for merging. Internally, Aviator automatically manages separate queues for each base branch. So if you have a PR #1 that is targeting base branch main and PR #2 that is targeting base branch release/v3.4, then those two PRs will not conflict with each other.

See the [GitHub docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches) for more information on working with branches.
