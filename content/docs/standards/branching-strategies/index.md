+++
title = "Branching Strategies"
description = "todo"
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

Branching strategies are generally applicable in two contexts.

* strategies used for development
* strategies used for releases

## Strategies for Development

Our preferred branching strategy is a style of **trunk-based development**. This means that in terms of collaboration we have that happen over the mainline branch (a.k.a. trunk) by following a workflow we developed to facilitate the creation of **small pull requests** and the highest quality peer reviews. You can read more about this in our post, [Journey to Small Pull Requests][].

Behind the scenes our workflow does use branches (**not** feature branches) when requesting review of patches. In turn enabling the peer reviewer to review in a manner that they would be used to. This also enables us to work with teams that don't follow our workflow and allows us to still use our workflow and get the values and benefits while still playing nicely with a team that is doing feature branches.

We don't believe in Feature Branches as outlined in [Journey to Small Pull Requests][] as they introduce a number of difficult problems in relation to writing quality code.

We also keep a linear [Git][] history through our workflow so that it is easy to go back and revert commits, etc. if necessary.

## Strategies for Release

Many people use a branching strategy for releases. For example they might have a **production**, **qa**, and **dev** branch mapping to respective environments. We do **NOT** use environment based branching or any other branching strategy for releases generally. We instead use [Git][] tags to manage releases.

This enables us to have and maintain our linear history while actually using immutable tags to represent releases and in turn trigger automated builds and deployments. In some cases we might also choose to do manually triggered releases.

Sometimes we are stuck in a client's environment in which they have built up a bunch of automation around environment based branching. In those case we generally recommend that client make their automation work in such a way that they don't have to merge branches back and forth and instead the branches can be force pushed to, triggering automation and deploys. This helps mitigate a bunch of confusion within the [Git][] history.

[Journey to Small Pull Requests]: /blog/journey-to-small-pull-requests
[Git]: https://git-scm.com
