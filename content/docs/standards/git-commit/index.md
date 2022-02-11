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

The Git commit is probably the most fundamental unit we have to manage the growth and evolution of our source code. Git commits can be extremely useful sometimes and other times they can be useless. The following are the guidelines we have honed in to over the years to help make sure that our commits end up being the valuable kind.

## A Logical & Purposeful, Buildable, Testable Unit

**Each commit should be a logical unit.** This means it should include only changes related to that logical unit. This includes related code changes, automated test changes, tooling changes, etc. This is **crucial** to suppoort the ability to easily revert changes when necessary. It also is crucial for aiding in doing any sort of git commit tree spelunking.

### Buildable

**Each commit should be buildable.** This means that each commit must NOT break the build tooling or build process of the application. This is important to make sure that all the Git tooling (e.g. git bisect) continues to be able to be used. It also more clearly suppoorts Git tree spelunking and hot fixing as no matter what commit you pick it should be in a buildable state.

### Testable

**Each commit should be testable.** This means that each commit must NOT break the automated testing tools or the tests. Similar to buildable this is important to make sure that all the Git tooling (e.g. git bisect) continue to work. It also helps with hot fixing, etc.

## Git Patch Stack

To help us follow these best practices we have developed a workflow on top of Git called [Git Patch Stack][] and built a small command Git extension to help streamline it. You can get a deeper understanding of this workflow and how to use it via our blog post, [How we should be using Git](https://upte.ch/blog/how-we-should-be-using-git/).

## A Communication Tool

Just as import if not *more important* than the source code change is the commit message itself. This is the only mechanism a commit author has to communicate very important things like, **what**, **intentension**, **reason (a.k.a. why)**, **approach (a.k.a. how)**, all crafted with **context**. Having this crucial information bound to the source code change is amazingly powerful and valuable when developing and maintaining an application over time.

### Official Git Commit Message Format

Git as a tool has guidelines for commit messages to work best with git and it's various tools. This format is as follows:

```text
Short summary of change (50 charecters or less in length)

One ore more paragraphs of text hard wrapped at 72 characters in length.
```

### Our Git Commit Message Format

We have built on the Official Git Commit Message Format to make sure we include this extremely important information. The following outlines our format.

```text
Short summary of change (the what) - must be 50 characters or less in length

One or more paragraphs explaining the INTENSION of the change with valuable
context. (Hard wrapped at 72 characters)

One or more paragraphs explaining the REASON for the change with valuable
context. (Hard wrapped at 72 characters)

One or more paragraphs explaining the APPROACH taken to attempt to achieve the
INTENSION or a portion of the INTENSION. (Hard wrapped at 72 characters)

A reference to the associated ticket identifer so that people can
quickly look it up and gain even more context.
```

### Examples

#### Definitely **NOT** what we are looking for

```text
Intergrate Sentry for crash handling
```

or even worse

```text
Update project.pbxproj
```

Picture someone throwing this commit over a wall to you with no other information and they asked you to review it. What would you actually be able to say about it other than what is already included in the diff. You could maybe call out a small syntactical, style, or maybe some logic things strictly based on the diff itself. But you wouldn't be able to provide any value in the peer review on if the changes meet the **intent** because you don't know what the **intent** is. You can't analyze the **approach** and provide valuable analysis of it, nor can you understand if the change is the right change to make to reach the over arching **reason** for the change. We don't even know from the short summary provided what in the project.pbxproj file changed. As a result you'd have to read the entire diff of changes and infer the intent, approach, and reason.

Making logical, purposeful, buildable, and testable commits with verbose commit messages may seem like overkill, but it becomes immensely useful for all the reasons stated in this guide, and more important when you no longer have access to the original developer who made the change! There are even benefits seen when the developer can't recall the intent, approach, and reason of changes that were made only a couple months ago.

#### What we are looking for

```text
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
	```text
	Short summary of change (the what) - must be 50 characters or less in length

	INTENSION paragraph(s) with context (Hard wrapped at 72 characters)

	REASON for the change with context (Hard wrapped at 72 characters)

	APPROACH taken to attempt to achieve the INTENSION (Hard wrapped at 72 characters)

	A reference to the associated ticket identifer so that people can
	quickly look it up and gain even more context.
	```
2. Configure Git to use your new commit message template
	```text
	$ git config commit.template ~/.gitmessage
	```
3. Make commits **without** (`-m`) and it will bring up the commit message template in your editor.

[Git Patch Stack]: https://github.com/uptech/git-ps
