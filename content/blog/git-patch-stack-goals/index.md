+++
title = "Git Patch Stack Goals"
description = "An exploration of the goals and intent behind the Git Patch Stack project."
date=2023-11-09
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/logo-git.png"
canonical = "https://drewdeponte.com/blog/git-patch-stack-goals/"
+++

*The following is a transcript of an audio recording I made exploring the goals
and intent behind the Git Patch Stack project. I have also included the audio
below if you would prefer to listen.*

<audio controls>
	<source src="git-patch-stack-goals.mp3" type="audio/mpeg">
Your browser does not support the audio element.
</audio>

## Transcript

Hi I am Drew De Ponte, Partner at Uptech Studio & creator of Git Patch Stack.

Today I wanted to talk about Git Patch Stack and the driving forces and purpose
behind it. Largely in my mind, the goal was and still is to find alignment
between Git itself as a tool, its characteristics, best practices and the
development process and tooling that we all generally use at mass today.

In my mind that is using Bitbucket, GitHub, GitLab and the pull requests or
merge requests peer review workflow that the mass is still using today. The
reason I was looking for alignment between these things was because I felt like
there was a dichotomy between the pull request feature branch workflow and Git
best practices.

How people were using feature branches in the feature branch workflow
at large was very much against the best practices of Git. One great example
of this is the atomic or logical commit being a best practice of Git. This is
because people would focus largely on feature branches as the primary concept
and they would ignore the value of commits.

They would ignore the benefits you gain longer term by having well structured
logical commits, and they would ignore the benefits you get in terms of all the
features of Git being available to you.

So Git Patch Stack has always been an exploration on my part to try and find
the best alignment between those two worlds. Most of our values have fallen out
of this exploration for this alignment. For example one of our values is that
we support the Continuous Integration Methodology as closely as possible.

We want to eliminate the negative side effects of branch isolation. So, We
always lean towards Continous Integration in terms of decision making. Having
the user confront conflicts and bring integration to the forefront so that it
happens at a much quicker iterative pace, and so that the surface area of the
conflicts are much smaller when they occur. Rather than waiting until a giant
feature branch is finally ready to be merged and having to deal with a
nightmare in terms of integration.

We are also big on aligning with Git. Hence why we focus on atomic or logical
commits and the tooling is designed around it. Not that you can't use branches,
because you can if you really want. Branches are fine. But you have to be a
very cognizant and aware of the consequences of those branches when you are
makeing those decisions. I think that's largely one of the downfalls that
occurred when Git came out.

Git itself is fantastic as a tool. It gives you all this flexibility and power.
However, when GitHub came out as a tool it created this next level of focus on
the concepts of the feature branch & pull request. Which only perpetuated heavy
use branches and drove developers to not think about the concept of logical
commits. Or even the fundamentals of Git anymore.

It's a higher level abstraction that they created & popularized. And most
developers nowadays come into Git through these higher level abstractions. I
can't tell you how many developers I've met that don't even know that there is
a tree of commits.

And that's a side effect of them learning from this higher level abstraction
and not learning the fundamental underlying abstractions. There's downsides to
that. Which is, without diving deeper and learning those underlying
abstractions, those developers just believe that, that's the truth.

They believe that, that is the only way. Or, that in actuality, they believe
that, that is the way it was designed and intended to be used, which is not
necessarily true. That's one person or one group of people's take on how they
think it should be used. And none of those people were the creator of Git.

They built that and put it out into the world and it resonated with people.
This makes sense because it's a simplification. Instead of thinking about
commits and how they relate to each other & all the complexities of the
mechanics of managing them in Git. You don't have to think about any of that.
You just think about this much grander, larger scale thing called a feature.
But that has its downsides as well.

So again, we as Git Patch Stack are focused on trying to find & and refine that
alignment between these worlds, where we can focus on the right things for Git
as a tool itself. But also play in a world with these higher level concepts
like feature branches & pull requsets, when we want or need to.

And also allow the user to, jump out and use a branch or any other normal Git
commads when it is appropriate. But the focus of the tooling is all on these
logical commits.

So we have this core value of, **abide by Git best practices**. That way we are
in alignment with what Git expects. **Support as much as possible the concept
of continuous integration**. So that we can reduce the surface area and
complexity of conflicts.

Third we have, **finding alignment with this concept of feature branches and
pull requests and peer reviews**. But implementing it in such a way, that you
aren't forced into the feature branch, pull request, or peer review concepts.

Instead, Git Patch Stack allows you from a workflow standpoint to plug into
those concepts, or it would allow you to, plug into an email based workflow
instead. When you conceptually request review in Git Patch Stack, there's a
hook system.

And that hook system allows you to plug in a hook to, go create a pull request
in GitHub, or go create a patch and email that patch to a particular mailing
list, or do anything you want. It's completely up to you.

But the goal of Git Patch Stack is to provide that core structure to think in
terms of the right concepts so that you are in alignment with all of these
things, not just GitHub's model.

Another value, is **being able to focus on how to mentally think about these
things and trying to do that with as little overhead as possible**.

You can see this point in terms of a comparison of the Stacked Branch workflow
versus a Patch Stack workflow. In a Stacked Branch workflow, you are creating
branches for each of your atomic commits, and then you're defining the
relationships between those branches so it can formally store a directed
acyclic graph of those relationships.

And then there's extra tooling both on the client side, where you manage your
changes locally, but also tooling on the application
side where it can read the directed acyclic graph and facilitate reviewing
pull requests in the particular order defined in the directed acyclic graph.
The DAG also enables additional automation in terms of management of these
branches and the DAG.

In my opinion, the Stacked Branch workflow's functionally solves some of the
problems that you have when you start thinking about atomic commits. But it
doesn't solve all of them. And it requires this overhead of defining and
managing the branches & the DAG in order to even solve the problems it does.

Again my goal with Git Patch Stack was to explore and find a way that things
fall into alignment with all of these things. That way developers can just
focus on the core concepts and principles & naturally be in alignment.

For example ideally, you don't want to have to worry about creating branches up
front and maintaining branches at all. Instead, the relationships of your
patches are managed through their order in your patch stack. And it's much
easier to think about those dependencies because that is the way those
dependencies end up in your actual tree later on.

The last value that I can think that we focus on is, and this isn't that you're
forced to do this but we do encourage it, that **you have a linear Git
history**.

If you think about it, the concept of a patch stack is a linear history itself.
And so the idea is that when you integrate, if you're integrating through Git
Patch Stacks tooling, you will end up with a linear history.

Now, technically Git Patch Stack is flexible enough that you can integrate
through GitHub, Bitbucket, or GitLab merges. If you do we recommend that you
configure it to do a rebase and merge so that you end up with a linear git
history.

But from a technical standpoint, you don't have to integrate that way. You
could do forced merges where you always have a merge commit, for example, where
you use `git merge --no-ff`.

It'll always create a commit even if it could fast forward. Some people prefer
to do that. Long ago in my career I believed that was the best approach because
I was focused on feature branches back then. However, now I no longer agree
with it.

So instead we push this idea of linear Git history. This helps all kinds of
things down the road. It helps you understand your Git history more quickly and
easily. It helps you understand and be able to revert logical changes from your
Git history.

I hope that provides some insight into what the goals are and why Git Patch
Stack exists. It's definitely continues to be a journey exploring and trying to
find, or now days more like, refine this alignment.
