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

We were struggling to deal with our pull requests. We weren't getting quality peer reviews of them, we were having a hard time reverting their commits when necessary, and even just having a hard time understanding the Git history because of them.

So we decided to put some thought into this and see what we could come up with.

The first thing was just understanding all the of the issues we were running into and coming to the conclusion that they are all a side effect of **big** pull requests. What we are calling **big** pull requsets are pull requests that have a changeset nearing a hundred lines of change or more. **Big** pull requests

* effectively **eliminate the value of peer review** as the changesets are just too overwhelming for people to focus on.
* silo changes from the team **delaying feedback**
* silo changes from the team **postponing integration**
* make **conflict resolution more difficult**
* encourage **poor git history & practices**
* make it **hard to revert changes**
* result in a **complicated git history** making it hard to look back and understand how best to interact with or even understand changes

## Why are they so big?

After understanding the that issues were coming from **big** pull requests. We decided we should try and understand why our pull requests were so big and see if there was a good reason for them being big.

In our case 


## How to make smaller pull requests


We should probably figure out how to make smaller pull requests. 
