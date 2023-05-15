# Git Subcommand Aliasing

You can map Aviator CLI command as `git` subcommand. In your `.gitconfig`, you can add aliases:

```
# ~/.gitconfig
# Do not forget an exclamation point before av.
[alias]
    sync = !av stack sync
    tree = !av stack tree
    pr = !av pr
```

With the config above, `git sync` will execute `av stack sync`.

This utilizes a feature that `git` provides. You can see the details in [man 1 git-config](https://git-scm.com/docs/git-config#Documentation/git-config.txt-alias).
