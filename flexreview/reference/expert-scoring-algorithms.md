# Expert scoring algorithms

## Expert scores

FlexReview’s reviewer suggestion and expert review requirements are based on expert scores. The score is calculated for every user and for every file based on the file modification history.

<figure><img src="../../.gitbook/assets/Untitled (3) (1).png" alt="" width="416"><figcaption><p>Public domain: https://commons.wikimedia.org/wiki/File:ForgettingCurve.svg</p></figcaption></figure>

This score calculation logic takes into account many factors. However, we are taking a simple approach that is based on the [<mark style="color:blue;">Forgetting curve</mark>](https://en.wikipedia.org/wiki/Forgetting\_curve). The forgetting curve shows how much information / memory is lost over the time in human’s memory. We apply this to the GitHub pull request code authoring and reviewing history. This allows us to build an estimator that reflects both recency and accumulated knowledge based on the past contributions.

While this is a very simple approach, the scoring gives us a good indicator. Recency gives us a powerful signal on person’s capability for code review; the most recent person who read and modified the code should have a good memory on what it does. However, accumulated knowledge is also an important factor. The resulting estimator is simple yet captures important aspects of expertise.

## Reviewer assignment

Based on the `CODEOWNERS` file and their expert scores, FlexReview can tell who can give an approval for each file. If we pick one reviewer for each file randomly or use load balancing, it can end up with too many reviewers, which can slow down the review time. FlexReview tries to minimize the number of reviewers assigned to a pull request.

<figure><img src="../../.gitbook/assets/Untitled (4) (1).png" alt="" width="375"><figcaption><p>CC BY-SA: <a href="https://en.wikipedia.org/wiki/File:SetCover.svg">https://en.wikipedia.org/wiki/File:SetCover.svg</a></p></figcaption></figure>

To minimize the number of reviewers, FlexReview utilizes the [<mark style="color:blue;">Set cover problem</mark>](https://en.wikipedia.org/wiki/Set\_cover\_problem). For a given set of files, each reviewer candidate covers some or all of those files. This can be directly mapped to a set cover problem.

Since this is known to be NP-hard problem, we use a greedy algorithm to get a close solution. This won’t give us an optimal cover, but it’s good enough for this purpose.
