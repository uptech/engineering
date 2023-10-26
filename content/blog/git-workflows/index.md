+++
title = "Git Workflows"
description = "An exploration of the Feature Branch, Classic Continuos Integration, Stacked Branch, and the Patch Stack workflows."
date=2023-10-26
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/logo-git.png"
canonical = "https://drewdeponte.com/blog/git-workflows/"
+++

*The following is a transcript of an audio recording I made exploring some of
the benefits and drawbacks of the Feature Branch, Classic Continuous
Integration, Stacked Branch, and Patch Stack workflows. I have also included
the audio below if you would prefer to listen.*

<audio controls>
	<source src="git-workflows.mp3" type="audio/mpeg">
Your browser does not support the audio element.
</audio>

*Note: I added some headers into the transcript to try and make it a tad bit
easier to jump to different sections if you are particularly interested in one
workflow.*

## Transcript

All right.

So let's talk about some common git workflows.

### Feature Branch Workflow

One of the most common git workflows is the Feature Branching workflow.

And I think one of the primary reasons why it's so common, is largely due to
the fact that GitHub was created and GitHub heavily promoted this workflow.
This of course is because GitHub is built around this workflows model.

It is worth noting, that just because it's the most common doesn't mean it is
the best workflow. It doesn't mean it's the worst workflow. It could be
somewhere in between. The truth is the commonality of it doesn't necessarily
indicate its value, quality, applicability to our needs.

We don't know until we actually reason about it. Comparing and contrasting the
benefits and drawbacks of these various workflows. And even then, we still
might not know definitively. But it will likely give us a bit more confidence
and definitely a deeper understanding.

So let's get into the Feature Branch workflow.

The way this works is that you have your git tree, and you have a main branch,
which is your main line of development.

Now, you don't work directly on that branch.

Instead, what you do is you create a feature branch based on it, to do your
work in.

And so in that feature branch, you make, all of your commits, all of your
changes, inside of that feature branch which is isolated away from all of the
code in the main line that's coming in from other developers.

So you're in this safe little isolated world where you can pretend that no
other changes are really happening for some period of time. Basically the
period of time that it takes you to develop that feature.

And then you have to work to get that feature branch merged back into the main
branch and pushed up to the upstream's main.

OK. Now how you go about this depends on your team dynamics and structure.

The way github facilitates this is via this concept they introduced called a
Pull Request. To use it you take your feature branch, you push your feature
branch up to the remote as a separate branch and then you open a Pull Request
which is effectively a merge request asking one of your teammates to review &
approve the merge of that branch into main line. This creates a Pull Request on
GitHub. And you can see the diff between the main line and the Feature Branch.

So you can see the changes made and people can comment on them and you can
discuss and debate whether those changes are good and you can iterate on your
feature and so on and so forth.

And then eventually, you know, once that's approved and good, then you can
merge that into main, and you can push main up to the upstream main.

GitHub helps kind of streamline the merging process a little bit by providing a
button to trigger the merge via the web interface.

OK. Now, so far, we've talked about a few things.

There's a couple of things that are nice and a couple of things that are not nice.

One of the things we've talked about is this convenient green button merge thing.

We talked about this idea of a Pull Request where you can see the changes and
that's also kind of nice.

But the other thing we talked about was this branch and this idea of
branch isolation. Which might seem like a nice thing initially, but you soon
realize that the longer you have a branch in isolation and you pretend that
there aren't other changes being made to the repository from other teammates,
you are increasing the probability that you will run into a conflict and you
are increasing the probability that the conflict when you run into, will be
larger and more complicated to resolve.

So that is a downside to this approach.

The other downside that's kind of hidden to this approach.

Or atleast less obvious, is that Git as a tool promotes this concept of having
logical small commits.

OK.

And logical commits means that they are a unit in themselves.

Another term related to this is called atomic commits.

Which can be conceptually thought of as the smallest, atom like commit, or
indivisible logical unit.

But fundamentally, the idea is that it's a small logical change that that
doesn't depend on other changes above it?

It only depends on changes that already exist in the git tree.

It's a logical change that is buildable and testable and everything the way you
want it. And that commits in general should be designed in this way.

So the Feature Branch workflow with GitHub has kind of led away from this idea.

What generally happens is that people don't take the time or think about the
commits themselves how they should be logically chunked.

In general people using the Feature Branch workflow don't tend to think much
about the commits at all.

Instead the developers tend to just think about the Feature Branch as this
logically chunked thing and they don't worry about the state of the commits or
whether they're logically chunked or not.

Now, the reason this is a problem is because the Git tool itself, and the
tooling that it provides is designed around these commits being smaller logical
commits.

So Git has features that are built around this concept logical commits, and if
you don't treat your commits that way, these features don't really work.

So one of those super useful features is called git bisect. It is extremely
useful when you're trying to isolate a bug by narrow down what code that
introduced it.

But there are other commands as well. Even when you do something like a git
blame and you're trying to understand why a change was was made. Having logical
commits is invaluable.

So the downsides of this Feature Branch workflow are that it kind of takes the
focus off of the commit and moves the focus strictly on to this idea of a
Feature Branch.

And therefore people naturally pay less attention to the commits.

Resulting in you losing the ability to use those vuluable tools.

Then we also have the other downside of it, which is the branch isolation.

When you have a branch isolated for that long, you increase the probability of
conflicts and the difficulty of those conflicts.

OK. So that's the Feature Branch workflow.

### Classic Continuous Integration Workflow

So let's talk about this other workflow I will call the Classic Continuous
Integration workflow.

The reason I call this the Classic Continuous Integration workflow is
because it is largely a workflow that people do when following the Continuous
Integration methodology.

It is a bit simpler than the branch based workflow because it doesn't really
deal with branches.

That doesn't mean you can't do branches with it if you want.

But in general you don't.

Instead, what you do is you make commits directly on your local main branch and
you pull in changes from upstream on a regular basis. Now when you pull these
changes in you do it in such a way that youl local main branch is rebased onto
the latest changes from upstream's main.

Once you get your commit on local main to a good place you integrate your
commit with upstream's main by pushing it up.

So it's much simpler and it lets you focus on the commits.

Which makes it easier to create logical commits and think about the commits.

So we are in alignment with the Git and their best practices and
expectations to support functionality within their tooling.

The reason it's called continuous integration is because this workflow is
driven off of people following the continuous integration methodology. Where
the idea is that you pull in changes on a regular basis and that you are
integrating your changes with upstream on a regular basis.

When we say a regular basis, we generally mean as quickly as possible.

This is because the the purest idea of continuous integration is that you and
your team are always working off of a shared single state.

Now, in reality, with the tooling today that's not completely possible.

But with the idea is that with continuous integration, you want to get as close
to that ideal as possible.

And so we have this, via this constant pulling and making a commit and
integrating it upstream and then pulling, making a commit, integrating it
upstream, etc. is fundamentally the flow that you have.

Now there this solved some problems, right?

For example we're now focused on the commit, the logical commit.

So we don't have that problem, we don't have branches.

We don't have to worry about branch isolation, which reduces the odds of
conflicts and their respective difficulty. This is because we're working closer
to that shared concept of shared truth.

And all of that's great, but it does have its drawbacks, right?

The rub comes when you're dealing with creating these logical commits on top of
main.

It requires you to do one of two things, to either write your code from an
inside out perspective.

Meaning you have to guess what's going to be needed as a dependency to build
the future feature you're trying to build and go do that inner most dependency
first, hoping that you've written its interface correctly and then integrate
that into upstream. And then the layer above it, and so on and so forth,
eventually to get to where your feature is complete.

But just hoping that your interface is correct, in my experience, is very
difficult. It is not very comfortable because you don't have confidence that
it's correct because you have no other software driving that development or
given you feedback about its correctness.

So that's one approach you can take, but that's very difficult.

The other approach you can take is to use a technique called Feature Toggling,
which allows you to integrate code into upstream that is incomplete.

So you can write code that is not complete, that is not currently being used
because you have a feature toggle that's turned off. But you can
integrate that code so that your other developers on your team are aware of it
and know about it and could build on it, provide feedback to you, or
provide guidance about how it might be used in another case, etc.

All of which are positive, useful things.

However, managing feature toggles is another layer of complexity, that has to
be dealt with.

Now managing that layer of complexity has a lot of other benefits.

Specifically Feature Toggles end up having a bunch of other benefits when
you start talking about things like release management.

So there's a ton of other benefits that you get that in my opinion are much
more worth the overhead than the complexity of branches which don't help you in
those realms at all. Branches only really help you with organization at the
workflow layer, but they don't help you beyond that.

One of the other side effects of this workflow, is that the peer review process
is very different.

The idea is that instead of opening a pull request and having your code
reviewed, you do post commit review, which is where you have committed and
integrated your commit upstream prior to any review by a peer.

And you're saying that is OK because it passes the build, and it passes all the
tests.

It is also been integrated into main and our automated CI tests have not
failed.

This is very different from the Feature Branch workflow where the review
happens prior to integration into upstream main.

Because in the Classic Continuous Integration workflow, the code gets
integrated into main prior to being reviewed by other developers.

You might be going well. This is a horrible idea. How could we do that? The
main is supposed to be this pure, perfect pristine thing.

The justification for this is simply the realization that you are lying to
yourself if you actually believe any code is perfect before it goes in. Just
think of how many times you have integrated a reviewed feature and then found a
bug or realized that it needed to be refactored to support something else.

The Classic Continuous Integration stance is simply accepting that main is this
ever evolving and changing thing and therefore adjusting it with a future
commit is no big deal as that is how we normally change it.

Now, if you talk about tooling Feature Branch workflow works great with GitHub,
Bitbucket, and GitLab. As they are all built around the same model.

But when you start talking about the continuous integration methodology, and
you start talking about this post commit review process, it's really difficult
to find off the shelf tools out there that focus on post commit review process

Most of them are kind of homegrown tools.

But yeah, so, that's the Classic Continuous Integration workflow.

### Stacked Branch Workflow

This brings us to the Stacked Branch Workflow.

The Stacked Branch workflow is trying to solve one of the complexities that I
talked about in the Classic Continuous Integration workflow while keeping the
focus on these atomic logical commits.

So if you remember, one of the hidden complexities I mentioned was that when
you're doing the Classic Continuous Integration workflow, you have to develop
your code from the inside out, or you have to use feature toggles.

So the way that the Stacked Branches workflow works is that you create a
branch, but each of your branches are really focused on one of these atomic
logical commits conceptually.

You're making a branch for each of these tiny logical changes that you're
making and then there's additional tooling on top of Git that creates this
directed acyclic graph (DAG) that understands the relationship between your
branches.

This allows you to create branches and then define the dependency relationships
between them, effectively building a stack (or sequence) of related (or
dependent) branches.

Having this directed acyclic graph and tooling that is aware of it enables the
tooling to automate what would be an extremely tedious process of rebasing all
the following branches when a lower level dependency branch changes.

It also gives other tooling the information to know in which order the branches
need to be integrated.

OK. So it's trying to focus on these logical commits.

It's not totally in alignment with the continuous integration methodology.

But it does facilitate when you pull, rebasing all of your stack of branches on
top of the latest main.

So you're at least locally integrating more frequently.

However, it fixes these problems by adding all of these layers of extra work.
Having to manage creating of branches, having to define relationships between
branches, having to rebase branches, etc.

It does enable you to build your code from the outside in though, which is good.

You still have to integrate your code in an inside out fashion or use feature
toggles.

The other downside to this approach is that because this isn't part of pure Git.
And it does not align with the current Feature Branch workflow of GitHub,
Bitbucket, or GitLab. You need additional tooling on top of that.

So your entire team has to switch to this additional tooling and this
additional application to see the branches, code review, and know the order in
which those branches need to be reviewed, etc.

OK. So that's the other major downside that even though it starts to solve one
of the core issues with the Classic Continuous Integration workflow.

### Patch Stack

So the last workflow that I'll talk about is what we call the Patch Stack
workflow.

As you might imagine from the name, this workflow is model around the concept
of managing a stack of patches.

Now this isn't a stack in the purest sense, you aren't limited to just push & pop operations.
Like you might think if you're thinking about a stack data structure. You can
reorder patches in the stack, put things in the middle of the stack, pull
things out of the stack, etc.

You can imagine it's just this mutable stack of patches that you can change and
mutate and do whatever you want with.

In this flow, you have a stack of patches which, are basically the commits that
you make on top of your local main that have not yet been integrated into
upstream main.

And when you do the get pull, it rebases your local stack of patches onto the
now updated origin/main.

So this meets the continuous integration needs because anytime you do a Git
pull, it is forcing you to integrate your local stack with the changes that
have been introduced by your other teammates.

Not only that, it focuses on this concept of patches or commits, which are just
logical commits.

These are the same small logical commits, which are also in alignment with
continuous integration.

Beyond that it allows you to develop your software in an outside-in fashion
while still doing the inside out integration similar to the stacked branch
workflow. Except in this workflow, you don't really concern yourself with
branches at all.

You don't deal with the overhead of branches.

Instead, you think about a stack of patches.

And if you want a commit to exist, that depends on another commit in your
stack, you just have to make sure that the commit that it depends on is lower
in the stack than the commit that you're working on.

This allows you to refine patches over time and work outside in.

But as you resolve and figure out what those inner most dependencies are, you
can integrate those right away like you would in a continuous integration
workflow or you could request review for them earlier to get them reviewed and
integrated into the code base as quickly as possible, which is also more in
alignment with the continuous integration methodology.

That sounds great.

It sounds like it's solving all the problems of the continuous integration workflow.

So let's talk about peer reviews though.

The Patch Stack workflow allows you to request review of an individual patch or
a series of patches.

And what happens behind the scenes is that this actually ends up mapping back
to branches.

When you request review of an individual patch, behind the scenes, it goes and
creates the branch, cherry-picks that patch into that branch, pushes that
branch up to the remote, and can even open the pull request for you on GitHub,
Bitbucket, etc.

This makes it so, it works with the existing tooling that's there. In turn
allowing you your team to use the same workflow they've always used and the
same tooling they've already been using.

But locally, you can develop your code using the Patch Stack workflow.

You might be asking yourself, wait, what about this dependency thing?

Do we have deal with all the complexity of managing branches locally and then
defining the relationships between those branches to define the relationships
between dependencies?

Nope, the Patch Stack workflow takes a different stance.

When you have dependent commits like that you have two options.

One is you request review for those individual patches one at a time in the
order that they need to be integrated and wait for the core dependent one to be
integrated before you request review of the next one.

Or you can create what's called a patch series and request review of a patch
series.

All that is, is saying, hey, I want to request review for these three patches
in series in this order they exist in the stack.

And that does the same thing behind the scenes.

It'll create a branch, it'll cherry-pick those patches in sequence into that
branch, push it up to the remote, and create a pull request for you based on
those patches going into upstream main.

So that's how the Patch Stack workflow deals with dependent patches.

You can have multiple patch series stacked on top of each other in your patch
stack.

You can also have individual patches inter weaved in there if you want. It
allows you to manage all of that directly within your patch stack, which is
just you focusing and thinking about logical patches.

So that's the Patch Stack workflow.

The benefits of the Patch Stack workflow is that it gets you as close as
possible to continuous integration but gives you the flexibility of defining a
patch series if you need to.

So you don't have that overhead of being required to manage these branches and
their dependencies between each other.

Beyond that, it works with the existing tooling.

So it works with the normal GitHub, Bitbucket, GitLab, etc. and their Feature
Branch based review process.

So you don't have to worry about any of this post commit review stuff, or the
tooling around that, or getting custom tooling made, or any of that stuff. You
just use the same tools you are likely already using.

In fact, another benefit of this is that it facilitates adoption more easily.

So let's say you as an individual are impressed with what you've seen from the
atomic commit community and want to start adopting those practices.

But your team is still working in a Feature Branch workflow.

Well, that's completely fine.

You can use the Patch Stack workflow locally and the rest of your team never
even has to know.

They don't care, they just see a Pull Request, and they do a review, the same
way they have previously for the Feature Branch workflow.

### Conclusion

So those are the four different workflows I wanted to talk about.

Hopefully, this helps clear up some of the concepts as well as some of the pros and cons of these four workflows.

Personally, I think the Patch Stack workflow makes the most sense.

Anyway, hope you enjoyed. Until next time.
