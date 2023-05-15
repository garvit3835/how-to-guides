# Optimizing CI execution

In parallel mode, fast forwarding or affected target mode, Aviator constructs draft PRs using a series of commits and creates temporary branches. These behaviors can cause your CI to run more builds than are necessary. To reduce the CI execution cost, you can optimize your CI config to skip runs based on certain conditions.

Aviator uses two separate branch prefixes:

* `mq-bot-*` - This branch prefix is used for draft PRs.
* `mq-tmp-*` - This branch prefix is used for temporary branches created just for final branch construction purposes, all CIs on these branches can be ignored.

### CircleCI

CircleCI natively supports ignoring specific branch prefixes. Update the workflow to exclude these branches:

```yaml
version: 2.1

workflows:
  version: 2
  build-and-test:
    jobs:
      - build-and-test:
          filters:
            branches:
              ignore:
                - /mq-tmp-.*/

```

### Buildkite

You can add mq-tmp-\* to the branch filtering at pipeline level. See [<mark style="color:blue;">Buildkite branch filtering</mark>](https://buildkite.com/docs/pipelines/branch-configuration).

### GitHub actions

GitHub actions does not support feature branch based filtering at workflow level. You can only filter by base branch, that does not impact the CI runs. Instead you can use filtering at job level:

```
name: main
on:
  pull_request

jobs:
  unit-test:
    if: startsWith(github.head_ref,'mq-tmp-') != true
    runs-on: "ubuntu-latest"
    steps:
      - name: run tests
        shell: bash
        run: |
          pytest
```

### Others

Similarly, for other CI systems, you can optimize the CI by blacklisting the mq-tmp-\* feature branches. The configuration may vary depending on how it is set up. If you have questions regarding a specific CI, please reach out to us: [<mark style="color:blue;">howto@aviator.co</mark>](mailto:howto@aviator.co).\
