+++
title = "Git Patch Stack v0.5.0"
description = "Announcing the official release of Git Patch Stack v0.5.0. This includes a deep dive on whats new and what has changed in this release."
date = 2020-12-10T08:30:01-08:00
updated = 2020-12-10T08:30:01-08:00
draft = false
template = "blog/page.html"
author = "Drew De Ponte"
+++

I just released [Git Patch Stack][] v0.5.0. To update simply run the following:

	brew update && brew upgrade git-ps

## What's Changed?

### Multiple Stacks of Patches?

This release makes it so that [Git Patch Stack][] now works with any branch that has an upstream branch, instead of having the patch stack be limited to the previously hard coded `master` & `origin/master` upstream branch.

#### Other Mainlines

This is useful for making [Git Patch Stack][] work with repositories that have other mainlines, e.g. `main` & `origin/main`.

#### Stuck in Feature Branches

It is also useful for managing branches that have upstreams in general. Lets say you are working with a client that doesn’t allow for individual patch pull requests but instead wants a feature branch with a series of logically chunked commits (a.k.a. patches, 😜). [Git Patch Stack][] can now be used to manage the patch stack within that feature branch.

### Removed Explicit Base Branch

This release removes the explicit base branch option from the `rr` and `pub` subcommands because I don't think it is needed now that [Git Patch Stack][] has dynamic upstream branch detection. Also, the explicit base branch option was confusing as it existed. If we find the need in the future to facilitate cross-upstream `rr`s or `pub`s we will add it back in a better way.

## Availability

This release of [Git Patch Stack][] is available via our Homebrew tap for both macOS Big Sur & macOS Catalina. See the [README](https://github.com/uptech/git-ps) for details this method and others.

[Git Patch Stack]: https://github.com/uptech/git-ps


