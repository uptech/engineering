+++
title = "Git Commit"
description = "A breakdown of how we think about Git Commits and what our standards are when creating them."
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 400
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

The Git commit is probably the most fundamental unit we have to manage the growth and evolution of our source code. Git commits can be extremely useful sometimes and other times they can be useless. The following are the guidelines we have honed in on over the years to help make sure that our commits end up being the valuable kind.

## Commit Characteristics

To aid with making sure our commits are the valuable kind and ensure they play nicely in the [Git Patch Stack][] and Continuous Integration methodologies we use the following characteristics as a checklist.

* logical
* buildable
* testable
* not-necessarily complete
* releasable

### Logical

**Each commit should be a logical unit.** This means it should include only changes related to that logical unit. This includes related code changes, automated test changes, tooling changes, etc. This is **crucial** to support the ability to easily revert changes when necessary. It is also crucial for aiding in doing any sort of git commit tree spelunking.

### Buildable

**Each commit should be buildable.** This means that each commit must NOT break the build tooling or build process of the application. This is important to make sure that all the Git tooling (e.g. git bisect) continues to be able to be used. It also more clearly supports Git tree spelunking and hot fixing as no matter what commit you pick it should be in a buildable state.

### Testable

**Each commit should be testable.** This means that each commit must NOT break the automated testing tools or the tests. Similar to buildable this is important to make sure that all the Git tooling (e.g. git bisect) continue to work. It also helps with hot fixing, etc.

### Not-Ncessarily Complete

**Each commit does NOT have to be complete.** This means that you could for example have a commit that introduces a new method to an existing class. That is a complete logical change that is buildable and testable. But it isn't complete in terms of its interactions with other application architecture concepts. This is really just a reminder that we should **NOT** be including tagentially related changes into our logical commits.

### Releasable

**Each commit should be releasable.** This means that once that commit is integrated into mainline. We should be able to cut a release of the application without a problem. So it shouldn't break the release process and it shouldn't leave the user experience of the application in a broken state. Most of the time you will find that commits can just naturally be releasable. But sometimes it means that you have to create initially unused alternate code paths or use feature flags/toggles to manage code paths so that code can be integrated into mainline while still keeping the commit releasable.

## Git Patch Stack

To help us follow these best practices we have developed a workflow on top of Git called [Git Patch Stack][] and built a small command line Git extension to help streamline it. You can get a deeper understanding of this workflow and how to use it via our blog posts, [Journey to Small Pull Requests][] and [How we should be using Git][].

## A Communication Tool

Just as import if not *more important* than the source code change is the commit message itself. This is the only mechanism a commit author has to communicate very important things like, **what**, **intention**, **reason (a.k.a. why)**, **approach (a.k.a. how)**, all crafted with **context**. Having this crucial information bound to the source code change is amazingly powerful and valuable when developing and maintaining an application over time.

We have found it is also a useful communication tool for some automation tasks. Specifically for including Changelog entry information so that we don't create unnecessary conflicts in the commit itself. [Git Changelog][] can then use this information to generate our Changelog files for us.

### Official Git Commit Message Format

Git as a tool has guidelines for commit messages to work best with git and it's various tools. This format is as follows:

```
Short summary of change (50 charecters or less in length)

One ore more paragraphs of text hard wrapped at 72 characters in length.
```

### Our Git Commit Message Format

We have built on the Official Git Commit Message Format to make sure we include this extremely important information. The following outlines our format.

```
Short summary of change (the what) - 50 characters or less in
length

One or more paragraphs explaining the INTENTION of the change with
valuable context. (Hard wrapped at 72 characters)

One or more paragraphs explaining the REASON for the change with
valuable context. (Hard wrapped at 72 characters)

One or more paragraphs explaining the APPROACH taken to attempt to achieve
the INTENTION or a portion of the INTENTION.
(Hard wrapped at 72 characters)

Associated Ticket Identifers

Git Changelog header and entries if applicable
```

### Examples

#### Definitely **NOT** what we are looking for

```
Intergrate Sentry for crash handling
```

or even worse

```
Update project.pbxproj
```

Picture someone throwing this commit over a wall to you with no other information and they asked you to review it. What would you actually be able to say about it other than what is already included in the diff. You could maybe call out a small syntactical, style, or maybe some logic things strictly based on the diff itself. But you wouldn't be able to provide any value in the peer review on if the changes meet the **intent** because you don't know what the **intent** is. You can't analyze the **approach** and provide valuable analysis of it, nor can you understand if the change is the right change to make to reach the over arching **reason** for the change. We don't even know from the short summary provided what in the project.pbxproj file changed. As a result you'd have to read the entire diff of changes and infer the intent, approach, and reason.

Making logical, purposeful, buildable, testable, not-necessarily complete, but releasable commits with verbose commit messages may seem like overkill, but it becomes immensely useful for all the reasons stated in this guide, and more important when you no longer have access to the original developer who made the change! There are even benefits seen when the developer can't recall the intent, approach, and reason of changes that were made only a couple months ago.

#### What we are looking for

```
Intergrate Sentry for crash handling

We did this because we previously had to get rid of MS AppCenter because
it wasn't compatible with mac Catalyst. The belief is that Sentry is
actually compatible and may be a better solution for us with the
need to support mac Catalyst. Also, living with out crash reporting is
just not smart.

This involved making a build configuration change to the Debug
Information Format changing it from `dwarf` to `dwarf-with-dysm` so that
we could also add a build phase script that would upload the build's
dsyms to Sentry so that it could symbolic the backtraces.

Beyond that we registered the sentry API tokens and enabled the crash
handler at app launch. Note: We did this with two different Sentry
projects, one for the mac Catalyst build and the other for the iOS
(iPad/iPhone) build. This is necessary because they are separate builds
that get deployed separately.
```

### Commit Message Template

If you are having difficulties remembering the git commit message format and what you need to provide in your message you are in luck. Git provides a commit message template feature. You can set it up with the follownig steps.

1. Create your template file in a safe place with the following content. Common practice is to put it in `~/.gitmessage`.
	```
	# Short summary of change (the what) ( <= 50 chars )

	# INTENTION of the change with context (Hard wrapped at 72 chars) 

	# REASON for the change with context (Hard wrapped at 72 chars)

	# APPROACH taken to attempt to achieve the INTENTION or a portion
	# of the INTENTION. (Hard wrapped at 72 characters)

	# Associated Ticket Identifers

	# Git Changelog header and entries if applicable
	# [changelog]
	# added: some addition you made that you want in your changelog
	# changed: some change you made that you want in your changelog
	# deprecated: some deprecation notice you want in your changelog
	# removed: some removal you want in your changelog
	# fixed: some fix you want in your changelog
	# security: some security fix you want in your changelog
	```
2. Configure Git to use your new commit message template
	```
	$ git config --global commit.template ~/.gitmessage
	```
3. Make commits **without** (`-m`) and it will bring up the commit message template in your editor.

[Git Patch Stack]: https://github.com/uptech/git-ps
[How we should be using Git]: /blog/how-we-should-be-using-git/
[Journey to Small Pull Requests]: /blog/journey-to-small-pull-requests/
[Git Changelog]: /blog/keep-a-changelog-without-conflicts/
