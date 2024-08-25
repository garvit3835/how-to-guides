# Updating default branch from master to main

If you choose to switch over the default branch, for instance from master to main, a few steps must be taken to update Stacked PRs CLI to handle this change:

### 1. Ensure that local remote HEAD is updated

You can do this with `git remote set-head origin --auto`.

You can read the git man page for more details:\
[https://git-scm.com/docs/git-remote#Documentation/git-remote.txt-emset-headem](https://git-scm.com/docs/git-remote#Documentation/git-remote.txt-emset-headem)



### 2. Modify .git/av/av.db file to replace the parent

In your repository `.git` directory has a file named `.git/av/av.db`. This file is a JSON document. If you open that, you can see sections like this:\
![image.png](https://mail.google.com/mail/u/0?ui=2\&ik=1eef997890\&attid=0.1\&permmsgid=msg-f:1808020807045977718\&th=191761583e7d9676\&view=fimg\&fur=ip\&sz=s0-l75-ft\&attbid=ANGjdJ8YPKnWnewOmP99DJh4WGLFT06mwtHrBUFje29aUY5CivAWcLWTwlOfpowinemqVZDVdf-MClOSaSXU6G687O40oIvZcvLS5LfH1kNHOkQy1sFIUtU4ASyreFg\&disp=emb\&realattid=ii\_m045ke640)\
\
This file is used by the `av` CLI to track each branch's parent. You can change `master` to  `main` or whatever default branch name change you are making for all existing branches. If you are unsure, please take a backup.

\
With these two steps, av should recognize main as master, including existing branches.
