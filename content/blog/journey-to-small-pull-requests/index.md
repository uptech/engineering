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

We were struggling to deal with our pull requests. We weren't getting quality peer reviews from our peers, we were having a hard time reverting commits when necessary, and had a hard time understanding the Git history.

So we decided to dig into this and see if we could get to the core of it and possibly come up with a solution.

The first thing was just understanding all the of the issues we were running into and coming to the conclusion that they are all a side effect of **big** pull requests. What we are calling **big** pull requsets are pull requests that have a changeset nearing a hundred lines of change or more. **Big** pull requests

* effectively **eliminate the value of peer review** as the changesets are just too overwhelming for people to focus on.
* silo changes from the team **delaying feedback**
* silo changes from the team **postponing integration**
* make **conflict resolution more difficult**
* encourage **poor git history & practices**
* make it **hard to revert changes**
* result in a **complicated git history** making it hard to look back and understand how best to interact with or even understand changes

## Why are they so big?

After understanding the issues were coming from **big** pull requests we decided we should try and understand why our pull requests were so **big** and see if there was a good reason for it.

We were following the [GitHub Flow][] and using feature branches, which in turn became our pull requests. Containing all the necessary changes for an entire feature turned out to be the primary reason that our pull requests were **big**. Not only that we noticed that developers weren't caring about the individual commits at all and would have partial or broken commits in their branches or even arbitrary save point commits. This all lended to the issues we had identified before.

## How to make smaller pull requests

So now we knew that if we wanted to our pull requests to be **small** we couldn't use feature branches. Instead we would have to scope our changes by a smaller concept than a feature. To help us identify which scoping concepts made the most sense we researched many other development workflow. The two that stood out as being different were the Continous Delivery Methodoly and Trunk Based Development as the others all seem to use feature branches.

Both the Continous Integration Methodology and Trunk Based Development are focused around getting small, logical, buildable, not-necessarily complete changes into mainline as quickly as possible. They both see this as a core principle to support other developers on the team being aware of the work being done and enabling contribution and planning around it.

Digging further into how people following these methodologies scope their changes. It seems to generally range from a change scoped to a single small addition of an architectural concept (e.g. add method to class) to the addition of a small/initial architectural concept (e.g. adding a small class).

One other difference is that people following these methodologies integrate code prior to it being utilized by other components. For example they would write a class and integrate that class prior to that class being used by whatever layers or features above it.

Beyond that they even go as far as using Feature Toggles to facilitate including code in the code base but preventing it from being executed. This allows all of the code to be intergated and shared with the development team as quickly and early as possible while still preventing consumers from seeing incomplete features.




[GitHub Flow]: https://docs.github.com/en/get-started/quickstart/github-flow
