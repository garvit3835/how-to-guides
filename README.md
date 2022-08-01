# Overview

![](.gitbook/assets/A\_Illustration.svg)

Aviator automates tedious developer workflows by managing git Pull Requests (PRs) and continuous integration test (CI) runs to help your team avoid broken builds, streamline cumbersome merge processes, manage cross-PR dependencies, handle flaky tests while maintaining their security compliance.

There are 4 key components to Aviator:

1. **MergeQueue** - an automated queue that manages the merging workflow for your GitHub repository to help protect important branches from broken builds. The Aviator bot uses GitHub Labels to identify Pull Requests (PRs) that are ready to be merged, validates CI checks, processes semantic conflicts, and merges the PRs automatically.
2. **ChangeSets** - workflows to synchronize validating and merging multiple PRs within the same repository or multiple repositories. Useful when your team often sees groups of related PRs that need to merged together, or otherwise treated as a single broader unit of change.
3. **FlakyBot** - a tool to automatically detect, take action on and process results from flaky tests in your CI infrastructure.
4. **Stacked PRs CLI** - a command line tool that helps developers manage cross-PR dependencies. This tool also automates syncing and merging of stacked PRs. Useful when your team wants to promote a culture of smaller, incremental PRs instead of large changes, or when your workflows involve keeping multiple, dependent PRs in sync.&#x20;

## How Aviator works

Aviator connects as a GitHub app that can be installed on any repository. The CLI can be installed using HomeBrew and configured using your GitHub personal access token. For our FlakyBot, you can also connect your CI provider so that Aviator can extract build/test logs from the provider.

Once installed, Aviator automatically monitors relevant activity happening in your repository, and based on the configurations you provide (via the Aviator webapp or Aviator config files), Aviator performs the required automated actions.
