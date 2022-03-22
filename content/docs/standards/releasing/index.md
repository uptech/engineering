+++
title = "Releasing"
description = "Our standards for releasing libraries, apps, etc."
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

Here at [Uptech Studio][] given that we use semantic versioning as our
[standard for versioning][] we can talk instead about the process of doing a
release.

## Normal release

Cutting a release consists of the following steps.

* identify the commit in the Git history that is to be the release - generally this is `HEAD`
* tag the commit with the new release version number, e.g. `1.2.3` or `v1.2.3`
* push tags up to the remote, e.g. `git push --tags`
* update the `CHANGELOG.md` if not automatically handled by CI, e.g. `git cl full > CHANGELOG.md` - *Note:* if this was an app with build numbers in the version you would use `git cl full -p > CHANGELOG.md`.
	* commit and push the update `CHANGELOG.md` commit for that version release

## Avoid Hot-fix release

We avoid "hot-fix" releases if we can. What this means in practice is that if we have a version that is released and we identify an issue with it. Instead of prepping a hot-fix release we just add the fix to the top of the tree and cut a new release including that fix as well as the other changes integrated into mainline.

This works because our [Branching Strategy][] is based on Continous Integration and Trunk Based Development which keeps our mainline in a build & releasable state.

## Hot-fix release

If for some reason we had to do a hot-fix release. There is nothing stopping us from being able to do it. We would have to simply perform the following steps.

* identify the tagged release that the issue is found on in our Git tree
* create a new hot-fix branch based on that tagged commit
* add a commit to that hot-fix branch to resolve the issue
* tag that commit with the new release version number
* backport that commit to mainline so the next release will also include it

## Automated Releases

We do **not** setup automated releases for libraries. Apps and Services on the other hand we do generally in the lower environments. But with some projects even in the production environment.

In the App case the automated releases are only used for internal release, bumping build numbers, and getting feedback from stakeholders. We generally use MS AppCenter for this.

In the Services case the automated releases are fine as build numbers as the interfaces are versioned within the code base.

## Marketing Releases

When we are doing a public Marketing Release of an application we go through the [normal release](#normal-release) process with tags and changelog, etc.

[Uptech Studio]: https://uptechstudio.com
[standard for versioning]: /docs/standards/versioning/
[Branching Strategy]: /docs/standards/branching-strategies/
