+++
title = "Git Patch Stack"
description = "An overview of Git Patch Stack and what we use it for"
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 420
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

Over the years we have determined that there are some pretty serious issues/drawbacks with how people classicly think of pull requests and the concept of **feature branches**, which seemingly & incorrectly have been intangibly linked for some time. Specifically, we have identified that the use of the feature branch concept tends to:

* promote non-logical commits, prohibiting certain Git functionalities (bisect, etc.) from working & making **valuable** peer review impossible
* promote extremely large pull requests, making **valuable** peer reviews impossible & time consuming
* promote lack of care around structuring commits which often results in people leaving WIP commits in pull requests
* have extremely heavy overhead in managing feature branches that depend on other feature branches
* often make the Git tree (history) extremely difficult to read & reason about

[Git Patch Stack][] is an open source extension to Git and methodology we built to addresses all of the above issues by removing the need for the concept of feature branches and introducing a concept and local development workflow we call, [Git Patch Stack][].

To understand why we developed this workflow and the journey we took to finally arrive at this workflow. Please see our post [Journey to Small Pull Requests][].

For more details on how to use Git Patch Stack you can checkout out our blog post [How we should be using Git][]. This provides the background and conceptual thinking around Git Patch Stack while also providing an example.

[Git Patch Stack]: https://git-ps.sh
[Journey to Small Pull Requests]: /blog/journey-to-small-pull-requests/
[How we should be using Git]: /blog/how-we-should-be-using-git/
