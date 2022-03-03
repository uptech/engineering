+++
title = "Journey to small pull requests"
date = 2022-03-02T09:32:01-08:00
updated = 2022-03-02T09:32:01-08:00
draft = true
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/logo-git.png"
+++

We all know that big pull requests, in the range of hundreds or thousands of lines of change, have a decent number of real problems.

* effectively **eliminate the value of peer review** as the changesets are just too overwhelming for people to focus on.
* silo changes from the team **delaying feedback**
* silo changes from the team **postponing integration**
* make **conflict resolution more difficult**
* encourage **poor git history & practices**
* make it **hard to revert changes**
* result in a **complicated git history** making it hard to look back and understand how best to interact with or even understand changes

## Why are we creating big pull requests?

If we know **big** pull requests result in all these issues and we think there must be a better alternative, maybe smaller pull requests. We need to figure out why we are creating such big pull requests in the first place.

