# Roadmap

## FlexReview Roadmap

There’s a lot to that goes into building an end-to-end code review experience from ground up. With that in mind, we would love to share our near-term roadmap for FlexReview. If you are excited about any of these features, please submit your request in this [<mark style="color:blue;">Google form</mark>](https://forms.gle/x1imtLv64LQ9mewN8). We are prioritizing features based on the interest.

The list below is in the order of priority.

## Chrome Extension

* See notifications for PRs needing your attention
* See suggested reviewers in GitHub UI
* Track who can review what files in a PR

## SLO automated actions

Configure automated actions based on the SLO window to improve code review response times. The rules can be set as default for the org and overridden at team level.

* X hours before SLO hit do Y if no review response
* X hours after SLO hit do Z if no review response

The actions could be one of:

* Relax the FlexReview approval requirement (does not need expert approval anymore)
* Notify the reviewer (Slack, AttentionSet, GH comments)
* Reassign review to someone else

## SLO timezone tracking

Track author’s and reviewers’ timezones for accurate calculation of SLO requirements.

## Teams

GitHub teams based rules that lets the team configure:

* different methods for selecting a reviewer
  * e.g. - expertise based, oncall, round robin, etc
* set up team Slack notification behavior
* set up notification for unattended PRs for the team members
* set up notifications for missing review SLO
* reassigning reviewer on missing review SLO

## Multiple reviewers

Certain changes / files may need to be reviewed by multiple reviewers before these can be merged. For example, some changes may require the feature team and security team to both sign off.

With multiple reviewers feature you can configure specific file(s) where two or more reviewers must approve for the changes to merge.

## Coaching mode

Prioritize the new members in the team for review assignment when the changes are authored by expert reviewers.

## Oncall schedules

Configure code review oncall schedules at the team level.

## Distributed config

Allow Codeowners style override configs to be applied at each directory level instead of one global config.
