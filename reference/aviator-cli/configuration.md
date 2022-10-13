# Configuration

## Configuration

The `av` CLI looks for a config file in a few places:

* `~/.config/av/config.yaml`
* `~/.av/config.yaml`
* `.git/av/config.yaml` (if running the `av` command inside of a git repository)

### Config option reference

{% code title="config.yaml (sample)" %}
```yaml
# Configuration that controls how av communicates with GitHub 
github:
    # REQUIRED (for `av pr create`)
    # The GitHub personal access token that av uses to authenticate to GitHub
    # You can create a token at https://github.com/settings/tokens.
    token: "ghp_abcdefghijklmnop"

    # The GitHub base URL to use.
    # Only set this if you use an on-prem enterprise GitHub installation
    # (leave blank if your repo is hosted on github.com)
    baseUrl: "https://github.com"

pullRequest:
    # If true, always open PRs as a draft.
    # If false, PRs will be opened as ready-for-review unless the --draft flag
    # is specified on commands that create pull requests.
    draft: false
    
    # If true, pull requests will be temporarily transitioned to draft state
    # while being rebased. This avoids accidentally adding unnecessary reviewers
    # to a pull request due to a CODEOWNERS file while the pull request is in a
    # transient state. This only applies when a pull request's base branch is
    # changing.
    # If not specified in the config, the default value is true if there is a
    # CODEOWNERS file present and false otherwise.
    rebaseWithDraft: false
    
    # If true, open a web browser to the pull request page whenever a pull
    # request is created for the first time.
    openBrowser: true
```
{% endcode %}

### GitHub Personal Access Token

The `av` tool uses a personal access token (PAT) to authenticate with GitHub on your behalf. This is required to create and inspect PRs (e.g., when using the `av pr create` command).

The generated token should have the `repo` scope (all other scopes can be left un-checked).

![Required permissions for the GitHub personal access token that you will use with av](<../../.gitbook/assets/Screen Shot 2022-05-26 at 11.20.35 AM.png>)
