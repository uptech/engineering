+++
title = "Journey to Small Pull Requests"
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

The first thing was just understanding all the of the issues we were running into and coming to the conclusion that they are all a side effect of **big** pull requests.

**Big** pull requests

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

So now we knew that if we wanted our pull requests to be **small** we couldn't use feature branches. Instead we would have to scope our changes by a smaller concept than a feature. To help us identify which scoping concepts made the most sense we researched many other development workflows. The two that stood out as being different were the Continous Delivery Methodology and Trunk Based Development, as the others all seem to use feature branches.

Both the Continous Integration Methodology and Trunk Based Development are focused around getting small, logical, buildable, testable, not-necessarily complete changes into mainline as quickly as possible. They both see this as a core principle to support other developers on the team being aware of the work being done and enabling contribution and planning around it.

Digging further into how people follow these methodologies and scope their changes. It seems to range from a single small addition of an architectural concept (e.g. add method to class) to the addition of a small/initial architectural concept (e.g. adding a small class).

One other difference is that people following these methodologies integrate code prior to it being utilized by other components. For example they would write a class and integrate that class prior to that class being used by whatever layers or features above it.

Beyond that they even go as far as using Feature Toggles to facilitate including code in the code base but preventing it from being executed. This allows all of the code to be intergated and shared with the development team as quickly and early as possible while still preventing consumers from seeing incomplete features.

## What about Peer Review?

One of our driving forces for looking at alternatives was to eliminate our problems with our peer reviews. Turns out that Continuous Delivery & Trunk Based Development don't provide much guidance on that front.

Many Continous Delivery & Trunk Based Development teams forego what we think of as a peer review process coming from feature branches. Instead they opt for getting the small, logical, buildable, testable, but not-necessarily complete changes into mainline as quickly as possible and then in theory go back and review each of the commits after the fact. This is known as **Post-commit** review. What we classically think of as peer review with feature branches is known as **Pre-commit** review. The difference is simply **Post-commit** happens after a commit is integrated into mainline while the **Pre-commit** happens prior to a change being integrated into mainline.

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

## Too Many Pull Requests?

One concern we had initially with this workflow was how the increase in the number of pull requests would impact things both on the producing pull requests side as well as the reviewing pull reqeusts side.

On the producing pull requests side we were pleasently surprised that we actually eliminated overhead. Before we used to have to write a pull request summary and description to give context about what the pull request is for and what it does in addition to all the commit messages. This went away because when you have a single commit branch for a pull request it grabs the commit summary and message as the pull request summary and description.

You could argue that there must be additional overhead in terms of the context that must be included in the commit messages. For us there wasn't as our [Git Commit Standards](/docs/standards/git-commit) already required us to write good commit messages that include the necessary context. If your team is coming from a place of not having good commit messages there will likely be a bit of overhead in terms of commit message writing. It is well worth it though and will pay dividends not only in supporting this workflow but also in supporting [Git][] history spelunking.

We were also pleasently suprised when we realized that we stopped caring about pull requests being reviewed immediately. It turns out that this is a wonderful side effect of having your patches in a stack. You can continue working on the feature without any branch overhead irrespective of it takes 30 mins or 2 days to get a review.

On the reviewing pull requests side of things the act of reviewing pull requests becomes a lot easier as everything you are reviewing is logically scoped, buildable, testable, etc. It allows you to really focus in on the change and how it plays out with the application architecture. It also makes it easier to allow your self to do some deep focus work as your team doesn't have the incessant need to ping you about their pull request to get it reviewed instantly.

There are definitely more pull requests. But when you come up for air from your deep focus you just tackle reviewing a few pull requests instead of one giant one. We have found that the organization and separation of these pull requests has only increased the speed and quality of our peer reviews. It is probably also worth mentioning that we use a product of ours, [Pullwalla](https://pullwalla.com), to identify which pull requests need review. This also helps us stay in deep focus.

## Outside-In & Inside-Out

In addition we were unsure exactly how things would pan out in situations where we have to go down a path to figure out how to implement something. Turns out that following best practices and doing outside-in development works great with this workflow. The key addition is that once we get to the inner most dependency we work our way inside-out requesting review of the patches.

Basically what ends up happening is that you create a bunch of logical WIP commits as you work from the outside-in. You do this until you get to the the inner most dependency (C) that has all the dependencies it needs. That patch (C) of the inner most dependency is what is fully baked and ready to have a pull request open. You are confident that your changes in C met the needs of B because you just walked through from the outside-in understanding the needs before you implemented C.

	A - WIP: Add ability for user to Login
	B - WIP: Add login ability to authentication service
	C - Add login() function to core underlying authentication library

Then you step back up to then next level above that (B) and you refine that patch updating it to use the stuff provided by C and make it no longer a WIP commit.

	A - WIP: Add ability for user to Login
	B - Add login ability to authentication service
	C - Add login() function to core underlying authentication library

At this point you open a pull request for B. Then you start working on refining patch A, using the bits that you implemented in B and get it to a good place.

	A - Add ability for user to Login
	B - Add login ability to authentication service
	C - Add login() function to core underlying authentication library

At this point you have a patch that technically implements the feature from a user perspective & adds the associated e2e tests. This is still a small, logical, buildable, testable, patch that you open a pull request for. The peer reviewer understand that this patch adds the feature and therefore when they review this pull request they do the full feature review as well as code review. Generally these inner patches are paired with unit tests while the outer most feature patch is paired with e2e tests.

There are many ways this can vary. For example with someone not doing outside-in development or maybe you have multiple internal dependency patches that aren’t intertwined. I think the important thing is just that the pull requests are opened from the inside-out.

Also one of the beautiful things about this workflow is that it decouples you from caring about how quickly your pull requests get reviewed because you are able to still continue development of your feature irrespective of how long it takes someone to review your pull requests. This happens thanks to the fact that the patch stack continues to hold the patches even after requesting review.

Further details on this process of outside-in and inside-out in relation to this workflow are covered in our article [How we should be using Git](/blog/how-we-should-be-using-git/).

## Ticketing Alignment

When we were doing feature branches before the ticketing alignment was trivial. We basically had a 1-to-1 mapping between a user story and feature pull request. With the Patch Stack methodology we had to make a choice. Do we have multiple patches reference a single user story. Or do we modify how we do ticketing so that it better aligns with the logical patches.

After thinking about it briefly we decided that it makes the most sense to just have multiple patches reference the same user story. Each of the patches could also reference any related sub-tasks that belong to the parenting user story. The key for this to be successful is really just providing the necessary context in the patch commit messages.

The alternate path of changing how we define and structure tickets seemed like a bad idea as it would have required not just engineering to change but also the product and project managers.

## Git Patch Stack

After using this workflow and seeing and feeling the value that **small pull requests** provide. We decided we could relatively easily streamline the little bit of branch overhead involved with this workflow and further enforce the conceptual thinking around patches by creating a simple [Git][] extension.

So we created [Git Patch Stack][] and it has only further solidified the workflow, the mentality around it, and the vast benefits that fall out of **small pull requests** and this workflow in general.

For a deeper dive on [Git Patch Stack][], this workflow and concepts to help identify these smaller logical chunks of work, see our post on [How we should be using Git](/blog/how-we-should-be-using-git/).


[GitHub Flow]: https://docs.github.com/en/get-started/quickstart/github-flow
[GitHub]: https://github.com
[Bitbucket]: https://bitbucket.org
[Git]: https://git-scm.com
[Git Patch Stack]: https://github.com/uptech/git-ps
