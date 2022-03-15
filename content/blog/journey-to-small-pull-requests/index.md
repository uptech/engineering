+++
title = "Journey to small pull requests"
date = 2022-03-15T09:32:01-08:00
updated = 2022-03-15T09:32:01-08:00
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/logo-git.png"
+++

We were struggling to deal with our pull requests. We weren't getting quality peer reviews from our peers, we were having a hard time reverting commits when necessary, and had a hard time understanding our [Git][] history.

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

We were following the [GitHub Flow][] and using feature branches, which in turn became our pull requests. Containing all the necessary changes for an entire feature turned out to be the primary reason that our pull requests were **big**. Not only that, we noticed that developers weren't caring about the individual commits at all and would have partial or broken commits in their branches or even arbitrary save point commits. This resulted in making reverting commits more difficult as well as breaking useful features of git like `git bisect`.  Beyond that it lended to the issues we had identified before.

## Alternatives

So now we knew that if we wanted our pull requests to be **small** we couldn't use feature branches. Instead we would have to scope our changes by a smaller concept than a feature. To help us identify which scoping concepts made the most sense we researched many other development workflow. The two that stood out as being different were the Continous Delivery Methodoly and Trunk Based Development as the others all seem to use feature branches.

Both the Continous Integration Methodology and Trunk Based Development are focused around getting small, logical, buildable, testable, not-necessarily complete changes into mainline as quickly as possible. They both see this as a core principle to support other developers on the team being aware of the work being done and enabling contribution and planning around it.

Digging further into how people following these methodologies and scope their changes. It seems to range from a single small addition of an architectural concept (e.g. add method to class) to the addition of a small/initial architectural concept (e.g. adding a small class).

One other difference is that people following these methodologies integrate code prior to it being utilized by other components. For example they would write a class and integrate that class prior to that class being used by whatever layers or features above it.

Beyond that they even go as far as using Feature Toggles to facilitate including code in the code base but preventing it from being executed. This allows all of the code to be intergated and shared with the development team as quickly and early as possible while still preventing consumers from seeing incomplete features.

## What about Peer Review?

One of our driving forces for looking at alternatives was to eliminate our problems with our peer reviews. Turns out that Continuous Delivery & Trunk Based Development don't provide much guidance on that front.

Many Continous Delivery & Trunk Based Development teams forego what we think of as a peer review process coming from feature branches. Instead they opt for getting the small, logical, buildable, testable, but not-necessarily complete changes into mainline as quickly as possible and then in theory go back and review each of the commits after the fact. This is know as **Post-commit** review. What we classically think of as peer review with feature branches is known as **Pre-commit** review. The difference is simply **Post-commit** happens after a commit is integrated into mainline while the **Pre-commit** happens prior to a change being integrated into mainline.

So we then started exploring the tools and processes around **Post-commit** peer review. Turns out that there isn't much there and the few tools that do exist are relatively antiquated and also require a full shift in terms of tooling and mentality. This was effectively a no-go for us as we work with many different clients and dev teams which all use [GitHub][]/[Bitbucket][] pull requests for peer review. So we needed to figure out a way to do **small pull requests** for **Pre-commit** peer reviews.

## Branches on, Branches, on Branches

Following our only path forward for peer review, **Pre-commit**, we started making **small pull requests** scoped down just as people do in Continous Integration and Trunk Based Development. This was great as we were seeing the benefits of **small pull requests** and see that it definitely does address the issues we identified at the beginning. However there was a ton of extra overhead and complexities in terms of managing these branches locally.

Generally we focus on implementing a specific feature/change. In order to do this we build up context around it in our mind so that we can plan and implement an appropriate change to achieve our goal. This is great when doing feature branches because there is a 1-to-1 mapping between that context and the branch. However, remember we are creating a number of pull requests that are small, logical, buildable, testable, and not-necessarily complete changes that when combined should accomplish the end goal of the feature/change. Continous Delivery and Trunk Based Development teams sometimes refer to these smaller changes as Proof of Work. Just like when you were a kid in school and the teacher made you show your work on your math homework. Each of the small changes should be logical steps showing how you achieved your end goal. You might think it is natural to just create a branch for each of these logical changes. However, what you quickly find is that the logical changes begin to have dependencies on other logical changes you have made.

This means for you to continue being able to develop the feature you effectively have to create your next branch on top of the previous branch and so on. You then end up with effectively a chain of branches composing up your feature. The only one you can actually open a pull request for is the inner most branch. This is because if you opened a pull request for any of the other branches it would also include the changes beneath it as well. Putting us back in the camp of **big pull requests** which we don't want to be in.

So we continued our experiment and dealing with the branch management overhead by accepting that it is ok that the inner most branch is the only one you can open a pull request for until it is integrated into mainline. However we quickly ran into the next issue. What happens when someone provides feedback on the pull request of the inner most branch and you have to make changes to it. Well, you have to make changes to it and then effectively go rebase each of the branches above it ontop of eachother one by one all of over again. This made the branch management overhead just too much to deal with.

## Eliminate Branch Management Overhead

So for **small pull requests** to be viable we determined we needed to find some way to eliminate or at least streamline all this branch management overhead. We pondered and struggled with this over time largely feeling like maybe it wasn't possible. However while researching development workflows of various teams we came across an old idea, the **patch**. Patches were how changes were managed and shared back in the day with many open source projects. A diff/patch would be created and emailed to the maintainer of the project. They would then review the patch and either apply it or reply back to the email requesting changes.

Teams that use patches still generally require that they are small, logical, buildable, testable, and most importantly **independent**. **Independent** doesn't mean that one patch can't depend on another patch. But it means that if the patch doesn't have to depend on another patch it shouldn't. This is generally addressed by the need to be **logical**, but having the additional explicit requirement of **independence** is worth thinking about. In the cases where a patch must have a dependency on another patch it is simply handled by communicating the required order in which those particular patches must be integrated.

Another interesting thing that maintainers do is locally manage a stack of patches they have recieved from contributors as they prepare changes to go into mainline.

This got our brains rolling again and got us thinking maybe if we added this requirement of **independence** and we start conceptually thinking about our commits on `main` as a stack of patches resting on `origin/main`. It would eliminate the branch overhead all together. Not only that, if we used the `pull.rebase = true` [Git][] feature it would force us to integrate our local stack of patches every time we pull. This would get us even closer to Continuous Integration and Trunk Based Development.

So now we had all the branch overhead gone and we are closer to Continuous Integration and Trunk Based Development than we have ever been before. But we needed to be able to do the equivalent of sending a patch to a maintainer for review but using **Pre-commit** review via pull requests.

Given that we are targeting **independence** and thinking about these commits as patches. It seemed natural when we wanted to request review of a patch to simply create a branch and cherry-pick the particular patch into that branch so that we could make a pull request from it. This worked and was far less overhead than all the branch management in the other path. Not only that it then allows [Git][] to help push us to **independence** by calling us out when we request review of a patch and it is **not** independent.

## Git Patch Stack

After using this workflow and seeing and feeling the value that **small pull requests** provide. We decided we could relatively easily streamline the little bit of branch overhead involved with this workflow and further enforce the conceptual thinking around patches by creating a simple [Git][] extension.

So we created [Git Patch Stack][] and it has only further solidified the workflow, the mentality around it, and the vast benefits that fall out of **small pull requests** and this workflow in general.

[GitHub Flow]: https://docs.github.com/en/get-started/quickstart/github-flow
[GitHub]: https://github.com
[Bitbucket]: https://bitbucket.org
[Git]: https://git-scm.com
[Git Patch Stack]: https://github.com/uptech/git-ps
