# Merge rules dashboard

Merge rules are at the core of how the Aviator bot behaves. MergeQueue primarily relies on GitHub Labels to communicate with pull requests. There are several attributes that you can customize for each repository using a yaml configuration file.

The simplest option for a quick setup is via the [<mark style="color:blue;">Merge Rules</mark>](https://app.aviator.co/github/rules) page.

{% hint style="warning" %}
Many advanced configurations are not available in the Dashboard UI and can only be configured through a configuration file. We only recommend using the Dashboard UI option if you require a simple setup.
{% endhint %}

The only required setting is the `Label for trigger` -  defaults to `mergequeue`. Once you add this to a PR, Aviator will queue and merge the PR.

![Label for trigger is the only required setting.](<../.gitbook/assets/Screen Shot 2022-05-23 at 5.43.35 PM.png>)

All of the settings in the UI are also covered in the Rules section of [<mark style="color:blue;">Configuration file</mark>](configuration-file/complete-reference-guide.md). Each setting also has a tooltip that provides more information.

#### Configuring Status Checks

See `Required Checks` under `Merge Preconditions` to set status checks for your PRs that need to be validated before merging. Selecting `Use GitHub mergeability check` will use all Github required tests for the repo.

<figure><img src="../.gitbook/assets/Screen Shot 2022-12-29 at 9.14.58 AM.png" alt=""><figcaption><p>required check configuration from UI</p></figcaption></figure>

If your team is using Parallel mode, by default, Aviator bot will use the same checks for original PRs and draft PRs. However, if you want to customize checks for draft PRs, you can do so under `Override required checks` in [<mark style="color:blue;">the Parallel Mode section</mark>](parallel-mode/).

![Override required checks for draft PRs.](<../.gitbook/assets/Screen Shot 2022-07-13 at 4.37.18 PM.png>)
