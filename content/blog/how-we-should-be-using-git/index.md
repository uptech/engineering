+++
title = "How we should be using Git"
description = "An exploration of how the Git creators use Git and how we should be using it."
date = 2021-01-26T01:32:01-08:00
updated = 2021-01-26T01:32:01-08:00
draft = false
template = "blog/page.html"
author = "Drew De Ponte"
+++

Git was created by Linus Torvalds out of a need. At the time the Linux Kernel team was using a proprietary Distributed Source Control Management (DSCM) system. However, due to licensing issues the Linux Kernel team could no longer use this proprietary DSCM system. Therefore, Linus decided to build Git as the DSCM system he always wished they had.

You might also know Linus as the creator of Linux. In fact, Linus still manages the Linux Kernel today. As of 2020, the Linux kernel had over 27.8 million lines of code spread across ~66 thousand files from ~21 thousand different contributors. It has continued to be successfully developed, maintained, and extended since it was publicly announced in 1991. It is also worth noting that Git itself, another large, successful open source project, is managed in the same way.

So there must be some useful insights around long term software development and maintenance practices we can glean by looking at how Linus and his team use Git for their development and peer review workflows.

## How the Linux Kernel Team works?

It may sound crazy but it turns out that the Linux Kernel and Git teams use email for both their code review process and their code submission process. They use a concept from the Unix days called a patch.

A patch is a simple text file that describes a change to another text file's content. In Unix there exists a tool called **diff** that takes two files and produces a patch file. People often refer to these patch files as *diffs*. Another Unix tool called **patch** takes a patch file and applies it to another file.

Back in the day this Unix patch concept is actually how open source development was done. The contributor would make a change to some source code, produce a patch, and email that patch to the maintainers requesting feedback. The contributor would then take the feedback and make the adjustments to the code. Then they would produce a new patch and email it back to the maintainers for further review and possible integration.

Fundamentally this is how peer review is done with the Linux Kernel and Git teams today. The primary difference is that they use Git to manage changes locally and then use a number of Git commands to generate a patch or a sequences of patches and email them to the maintainers. They also use a number of Git commands to consume and apply patches from emails. See `git apply`, `git am`, `git format-patch`, `git imap-send`, and `git send-email`.

In addition the team also requires that patches have certain characteristics.

- a patch must be **small**, **logical**, **independent**, **buildable**, & **testable**
- a patch must provide a clear description of what changed, why it was changed, and the necessary context of how it is or will be used
- patches must be submitted individually for review

If we dig a bit further into the Linux Kernel team's process we find that they have integrators responsible for the various subsystems. These integrators receive numerous patches from various contributors and manage them locally as a series of patches (using **quilt**, another Unix command). This involves reordering, updating and dropping them on a regular basis. Linus then regularly pulls the changes from the various subsystem integrators into the upstream repository's mainline.

## What did we learn?

From a small dive into the Linux Kernel team's development process we can make three important observations.

First, the process imposes a strict set of requirements for each patch before it may be sent for review or submission. Second, the team's process works well over email, a very asynchronous communication mechanism. Finally, they locally use the concept of managing a series of patches while preparing changes for integration into upstream.

## So what's the Magic?

Is it the Kernel team's avoidance of powerful peer review tools that makes them so successful? Or the fact that they do this all over email?

I think not. I believe their success is primarily due to their process. The specific requirements for patches are fundamentally in alignment with the core design principles of Git. They are also structured around the concepts of boundaries of the application architecture.

Beyond that, it seems this mental model of locally managing a series of patches as they are prepared to be submitted for review or submission is key.

## Outside-In

Let's assume for the time being I am correct and we trust that following these rules and this mental model will greatly benefit us. (Later we can explore the actual values we get from these rules and why they are so important.) How can we use this to benefit our own projects?

Before we can apply these rules we need to first understand how to break down our code changes into small logical independent chunks that are buildable and testable. To help us with this, let's introduce a new mental model for thinking about application architecture in relation to boundaries, called *Outside-In*.

With Outside-In you begin your approach to code changes by starting from the outermost (UI/UX) layer of the architecture, and drive layer-by-layer down into the architecture until you reach your core dependency, the inner most layer of the relevant architecture; you then reverse direction and work your way out again, wiring the layers together as you go.

![Animation showing Outside-In development](outside-in.gif)

This method ensures that you are building exactly what you need-nothing more, nothing less. It also helps as a tool to recoognize different layers of abstraction within your application which aids massively in thinking about the overall application architecture. Last, it also drives the interfaces of your abstracitons so you end up with more clearly defined boundaries.

## Sounds great in theory

But what does this actually look like?

Let's say we are tasked with adding the ability to login to our mobile app. If we follow Outside-In this might look something like the following series of steps:

1. add login view (incomplete because depends on a API client that doesn't exist)
2. add login support to API client (incomplete because depends on REST API lib)
3. add lib to facilitate making REST API requests (our core dependency)
4. complete add login support to API client (wiring it up to the lib added in the prev step)
5. complete add login view (wiring it up to the API client login ability added in the prev step)

If we created commits locally for each of these steps our local commit tree would look like the following:

```
* complete add login view (HEAD, main)
* complete add login support to API client
* add lib to facilitate making REST API requests
* WIP: add login support to API client
* WIP: add login view
* ... (origin/main)
```

This is fine, but these don't look like logically chunked commits. They are also in the incorrect order based on dependencies.

Luckily we are working in Git, which provides the interactive rebase (`git rebase -i`) command which facilitate reordering and squashing commits together, amongst other valuable things.

Before we fix these issues, let's just look at what the tree would look like if we reordered it based on dependencies and didn't squash the commits.

```
* WIP: add login view (HEAD, main)
* complete add login view
* WIP: add login support to API client
* complete add login support to API client
* add lib to facilitate making REST API requests
* ... (origin/main)
```

From this ordering we can start to actually see the relation and the logical chunking. For example the "WIP: add login view" and the "complete add login view" are two commits that are part of the same logical chunk.

If we identify these commits and squash them together into their respective logical chunks we would end up with a local Git tree that looks as follows:

```
* add login view (HEAD, main)
* add login support to API client
* add lib to facilitate making REST API requests
* ... (origin/main)
```

The Kernel team describes a series of patches as a "proof of work". Think back to when you were a kid in school and your teacher required you show how you got to a solution for each problem. This concept is effectively the same thing. Each logical patch in the series is a step of you showing how you are solving the problem. This is a another great mental model and is useful for making sure your series of patches make sense.

## Where are we with these rules?

At this point we have figured out how to locally iterate on commits to get them to a point where they effectively are a series of **small**, **logical**, **independent**, **buildable**, and **testable** patches.

That leaves us with the following:

- a patch must provide clear description of what changed, why it was changed, and the necessary context of how it is or will be used
- patches must be submitted individually for review

Meeting the first of these is simply writing a good commit message. Check!

But how do we follow the second requirement to submit patches individually?

## Submit for Review or Integration

We could, of course, setup a mailing list like the Linux Kernel team and use the same Git commands they do. However, this seems archaic when we have tools like GitHub, Bitbucket, Gitlab, etc. that provide the concept of a Pull Request, as well as code review tools that are much nicer than email. Not to mention the fact that the majority of the development community is already comfortable with these tools due to the unjust popularity of Feature Branch development.

So how do we create a pull request of an individual patch once it is ready for review?

1. create a branch based on the current branch's upstream remote branch (e.g. `origin/main`)
2. cherry pick the patch you want reviewed to this new branch
3. push this branch up to the remote (e.g. `origin/this-patches-branch`)
4. create a pull request from this new branch (`origin/this-patches-branch`) into upstream mainline (`origin/main`)

Now reviewers can review the changes as they normally would. They will have a clear description of what changed, why it was changed, and the necessary context of how it is or will be used, and will be able to easily review the actual code, testing them out, and providing feedback.

A little later we will introduce a tool that automates these steps for you, making this quick and easy.

## So What's the point?

Interestingly, it turns out that structuring your patches (a.k.a. commits) logically around the concepts in your application architecture is a major part of the magic. This specifically aids with understanding concepts, how they are introduced, and how they are modified over time to support higher level concepts and features. This is what largely facilitates **long term maintenance of software**.

This method also facilitates **requesting review for individual patches as soon as they are completed**. I can't emphasize how important this is. This enables us to basically develop Outside-In but request review Inside-Out. Meaning as soon as the inner most dependency is complete we can request review of that and get feedback all while we are still working on the layers above it.

Beyond that these small logical, buildable, testable patches result in **a faster mean time to repair**, **extremely focused and valuable reviews**, **support for fault isolation**, **decreased code review time**, and **promote test reliability**. They also facilitate **integrating patches sooner**, which in turn **promotes sharing**, **refactoring**, **discovery**, and **natural evolution** of application architecture concepts. Not to mention helping to **prevent duplicate efforts** and **improve the overall release rate**.

Again these small logical, buildable, testable patches fall in line with Git's principles of design and in turn help to **unlock the full powers of Git**. Specifically in Git there are certain extremely powerful commands (e.g. `git bisect`) that only function well if your Git tree is composed of logical, buildable, testable commits.

The concept of a **series of patches** also ends up having a huge benefit of **eliminating urgency around having one's pull requests reviewed immediately**. This is because even after you have submitted a patch for review, you still have that patch in your series of patches locally and can continue building on it without issue. This significantly aids in reducing interruptions between developers, breaking their focus, and inevitably making things take longer. This can't be obtained using feature branches without huge overhead and it is a key differentiator in supporting asynchronous workflows.

How do you make sure people are aware of new pull requests without interrupting them? Don't worry-this is a problem that all pre-review based development methodologies suffer from. To help address this issue we built [Pullwalla](https://pullwalla.com), a unified pull request manager. 

Finally, following these rules also results in your Git commits and in turn your Git tree representing "proofs of work", the mental construct the Kernel team uses to vet a series of patches. Turns out these "proofs of work" provide an immense amount of guidance and value in simply understanding a code base's application architecture and the intentions behind the various logical changes.

## Patch Stack

Given that we have found this development methodology to be unmatched in value, we decided we must formalize it a bit more. We decided to call this the **Patch Stack** methodology, as it resembles a stack of papers that can be reordered and modified.

Achieving the full value of the methodology requires that these rules are followed and that you locally manage your commits as if they are conceptually a stack of patches being reordered and modified in preparation for submitting for review or integration.

- a patch must be **small**, **logical**, **independent**, **buildable**, & **testable**
- a patch must provide a clear description of what changed, why it was changed, and the necessary context of how it is or will be used
- patches must be submitted individually for review

In addition to formally defining the rules, we have built [git-ps][], an OSS Git command line extension to facilitate making this workflow and methodology even easier to use.

## Getting the Feel for It

To get a better feel for how this methodology works in real life let's walk through our example from before. If you recall we are tasked with adding Login ability to a mobile app.

### Where do we start?

We take guidance from Outside-In and start at the outer most layer of our application architecture (UI/UX). We first look at the existing UI layer of the application architecture to find out if it provides any support for login.

Turns out it doesn't. So we are off to the races, building out a login screen and adding it to the flow of the app. We get to the point where we are attempting to wire up the submit button of the login screen and realize our client API for our backend doesn't support login.

So we figure out the ideal interface that should get added to the our client API to facilitate login. We also implement code to use this imaginary interface. This code obviously won't build and isn't complete, but we include it anyways as it acts as a note to ourselves for what the ideal interface should be.

Given we are following Outside-In we also recognize that we are at a boundary between application architecture concepts, specifically the UI and the client API. This is our clue that we need a patch to house this logical (UI) piece of the puzzle.

So we add a patch on top of our stack by first staging the UI changes, `git add .`, and then creating the patch.

```
git commit
```

When it prompts for the commit message we provide something like the following, making sure to provide a clear description of what changed, why it was changed, and the necessary context of how it is or will be used.

```text
WIP: Add login screen

So that users will be able to login to their account.

This interfaces with the client API to actually perform the login and get the
authentication token.
```

We used the convention of prefixing our commit summary with "WIP:" as a reminder to us that this patch is still a Work In Progress.

### Onto the Client API

Now that we have created our WIP patch for the UI layer. We move onto to the next layer down, the client API.

So we extend the client API by adding our ideal interface and starting to implement the functionality behind it, making any necessary adjustments to the interface as we go.

Soon we realize that we don't have a library to facilitate interacting with the REST API. We also recognize this as yet another boundary in the application architecture. Therefore, we create another patch on top of our stack by staging the changes, `git add .` and then committing.

```
git commit
```

We provide the following message with the patch.

```
WIP: Add BackendClient.login(username:password:) method

So that our application has the ability to make the login request to the
Backend REST API given the username and password.

This is intended to be used by the login screen to help facilitate performing
the actual login. It will be interfacing with some REST API to actually make
the REST call.
```

### What's our Patch Stack look like?

We have added a couple patches to our Patch Stack and are ready to check where we are in our process. To do this we simply list our patch stack using the following.

```
git ps ls
```

It outputs a view of all the patches on our stack.

```
  1     23d7a6 WIP: Add BackendClient.login(username:password:) method
  0     af73d2 WIP: Add login screen
```

Things look good and we are reminded that we just added the `BackendClient.login()` method and it is incomplete because of a missing dependency, the REST API library.

### Onto the REST API library

So we do some research and find what we think is the best REST API library and add it to our `Package.swift`, making it available to our app.

This is yet another boundary in our application architecture and therefore we create a new logical patch for it by staging the changes, `git add .`, and committing.

```
git commit
```

With the following message:

```
Add Siesta, the elegant way to write REST clients

So that we can easily and most importantly make elegant REST requests.

We intend to use this inside of our BackendClient.login(username:password:)
method to elegantly make the login REST API request.
```

It is worth noting that we do **not** use the "WIP:" prefix in our patch summary here. This is because it is a complete independent logical patch and happens to also be our core dependency in the context of Outside-In.

### Where is our Stack at?

Let's check out our stack of patches now.

```
git ps ls
```

It outputs the following.

```
  2     4387d1 Add Siesta, the elegant way to write REST clients
  1     23d7a6 WIP: Add BackendClient.login(username:password:) method
  0     af73d2 WIP: Add login screen
```

### Reorder our Patches by Dependency

Things are looking pretty good but our patches are not ordered correctly based on dependencies. Let's fix this by reordering them using `git ps rebase` and changing them into the following order.

```
pick 4387d1 Add Siesta, the elegant way to write REST clients
pick 23d7a6 WIP: Add BackendClient.login(username:password:) method
pick af73d2 WIP: Add login screen
```

This order is correctly ordered by dependency, as "Add Siesta..." is the inner most core dependency, followed by "WIP: Add BackendClient.login..." being the layer that directly depends on "Add Siesta...". Following that is the "WIP: Add login screen" patch which depends on "WIP: Add BackendClient.login...".

### Pull Changes from Upstream

At this point we have a logical, buildable, testable patch that is ready to be submitted for review. However, lets make sure that our patch stack is good with all the other work people have recently published to upstream. We do this as follows.

```
git ps pull
```

This takes the stack of patches and essentially lifts them up and replays them on top of the upstream changes.

The `git ps pull` ran successfully with no conflicts. So all the patches in our stack are good.

```
  2     af73d2 WIP: Add login screen
  1     23d7a6 WIP: Add BackendClient.login(username:password:) method
  0     4387d1 Add Siesta, the elegant way to write REST clients
```

### Request Review


We now request review of our "Add Siesta..." patch as it is a complete logical patch and we want people to be able to review it while we are working on implementing the rest of our feature.

```
git ps rr 0
```

In the case where we initially request review of a patch with GitHub it will dump the URL that creates the pull request to the command line. That way all you have to do is click that URL to create the pull request.

Getting feedback on this inner most dependency first is important as the architectural decisions around it can impact the work in the layers above.

### Back to the Client API

One of the benefits of working in a stack of patches is that we can continue building on top of our previous patches even after we have requested review of them.

Now that we have the Siesta library available to us, we go back to our `BackendClient.login(username:password:)` method and finish implementing it using the Siesta library.

Once we are done we create a new patch on our stack using `git commit -m "finish BackendClient.login(username:password:)"`. *Note*: We don't care so much about this patch message as it is a temporary patch. We just need to know what it does in relation to the other patches in our stack.

If we checkout our stack of patches with `git ps ls` it looks something like this now.

```
  3     df71da finish BackendClient.login(username:password:)
  2     af73d2 WIP: Add login screen
  1     23d7a6 WIP: Add BackendClient.login(username:password:) method
  0 rr  4387d1 Add Siesta, the elegant way to write REST clients
```

### Reorder & Squash

Looks like we have the makings of another logical patch. However, it is made up of two separate patches, the "WIP: Add BackendClient.login..." patch and the "finish BackendClient.login..." patch.

Lets reorder these patches and squash them together using `git ps rebase` to turn them into a single logical patch.

```
pick 4387d1 Add Siesta, the elegant way to write REST clients
pick 23d7a6 WIP: Add BackendClient.login(username:password:) method
s df71da finish BackendClient.login(username:password:)
pick af73d2 WIP: Add login screen
```

When we save and quit the editor it will open the squashed patch descriptions up to create the singular patch message. We remove the temporary patch message and remove the "WIP:" prefix from the summary as this is now a complete, buildable, testable, logical patch.

If we list our patches with `git ps ls` again. Our patch stack now looks as follows.

```
  2     af73d2 WIP: Add login screen
  1     23d7a6 Add BackendClient.login(username:password:) method
  0 rr+ 4387d1 Add Siesta, the elegant way to write REST clients
```

### Review Feedback

We just got a notification that someone reviewed our first patch and requested some changes. Instead of using *Siesta* as our REST library they want to use *Alamofire* and have some decent arguments for doing so.

So looking at our stack of patches we need to edit the "Add Siesta..." patch to change it to Alamofire.

We can accomplish this by using `git ps rebase` and changing the "Add Siesta..." patches rebase command from `pick` to `edit`. When we save and quit the editor it will drop us into being able to edit the patch we marked with `edit`.

So we make the necessary changes, removing Siesta from the `Package.swift` and adding Alamofire.

We then stage these changes with `git add .` and amend them to the "Add Siesta..." patch using `git commit --amend`. This in turn opens our editor with the patch message allowing us to update it.

```
Add Alamofire as a REST client

So that we can easily make REST requests.

We intend to use this inside of our BackendClient.login(username:password:)
method to make the login REST API request.
```

Once we have saved and quit the editor the patch has been updated. However, we still need to complete the rebase which stopped in the middle.

To do this we simply run `git rebase --continue`.

### Re-Request Review

If we checkout our patch stack it should look something like this.

```
  2     af73d2 WIP: Add login screen
  1     23d7a6 Add BackendClient.login(username:password:) method
  0 rr+ 4387d1 Add Alamofire as a REST client
```

The `rr+` is indicating that there have been changes made to that patch since we last requested review of it. So we need to re-request review so that the reviewer can see the recent changes we have made.

We accomplish this by simply running `git ps rr 0` again.

This will update the previously created pull request for this patch so that the reviewer and can re-review it.

### Rework BackendClient

Now that our core dependency is ready for review again we can focus on the layer above that again. Since our lower layer dependency changed we need to update our "Add BackendClient.login..." patch to use Alamofire instead of Siesta.

To do this we simply use `git ps rebase` again and mark the "Add BackendClient.login..." patch for `edit`. It drops us back to the console waiting for us to edit that patch.

We do so by making the necessary changes, staging them with `git add .`, and then amending them with `git commit --amend`. When prompted to the update the message we simply save and quit the editor because the message is still correct.

Then we run `git rebase --continue` to have it finish the rebase process.

Leaving our patch stack as follows.

```
  2     af73d2 WIP: Add login screen
  1     23d7a6 Add BackendClient.login(username:password:) method
  0 rr+ 4387d1 Add Alamofire as a REST client
```

### Request Review of the 2nd patch

Now that we have our second patch complete. We pull the upstream changes again with `git ps pull` and then request review of our second patch with the following.

```
git ps rr 1
```

Then we click the URL to create the pull request, making it ready for feedback and leaving our stack of patches in the following state.

```
  2     af73d2 WIP: Add login screen
  1 rr  23d7a6 Add BackendClient.login(username:password:) method
  0 rr+ 4387d1 Add Alamofire as a REST client
```

### Approval Arrives

Meanwhile the approval notice has come in for our "Add Alamofire..." patch.

So we publish this patch upstream by running the following.

```
git ps pub 0
```

This will push that patch up to the remote branch leaving our stack of patches in the following state.

```
  2     af73d2 WIP: Add login screen
  1 rr  23d7a6 Add BackendClient.login(username:password:) method
  0 p   4387d1 Add Alamofire as a REST client
```

### Pull and Collapse

We decided to pull in any changes from upstream with `git ps pull` making our patch stack now look as follows.

```
  1     af73d2 WIP: Add login screen
  0 rr  23d7a6 Add BackendClient.login(username:password:) method
```

You will notice that the "Add Alamofire..." patch has left our patch stack. This is because it has conceptually moved from our stack of patches into its upstream branch.

### Back to the UI

We have now worked our way back up to the outer most layer of our application architecture, the UI. Here we make changes to wire the submit button of the Login screen up to the `BackendClient.login(username:password:)` method completing this feature.

Since our "WIP: Add login screen" patch is the top most patch. We can simply amend these changes to that patch by staging the changes with `git add .` and doing the following:

```
git commit --amend
```

This opens the patch message in our editor and allows us to remove the "WIP:" prefix from the patch summary.

If we list our patch stack with `git ps ls` it should look as follows.

```
  1     af73d2 Add login screen
  0 rr  23d7a6 Add BackendClient.login(username:password:) method
```

Then we integrate upstream changes with `git ps pull` and request review of our final patch using `git ps rr 1`.

Leaving our patch stack in the following state.

```
  1 rr  af73d2 Add login screen
  0 rr+ 23d7a6 Add BackendClient.login(username:password:) method
```

### Onto the Next

At this point we have requested review of all our patches for this feature and are waiting for feedback. However, we don't just sit idle, waiting for the feedback. We naturally start our next task in the same manner, Outside-In, and adding, reording, and updating patches on top of our stack.

You might be saying to yourself, "Hey, but we have this other feature's patches here." You are correct we do. This is fine and in lots of cases it is actually more beneficial to continue building new features on top of the ones you have locally. And if for some reason you find out that this new feature has priority over the one that was just completed. Simply reorder the patches in your stack to represent that.

### More Acceptance

Over the next couple hours reviewers find time to review the other two patches and they are both approved.

We publish them using `git ps pub 0` and `git ps pub 1` changing our patch stack state to the following.

```
  1 p   af73d2 Add login screen
  0 p   23d7a6 Add BackendClient.login(username:password:) method
```

After publishing these patches we run `git ps pull` to integrate any upstream changes. In this case it includes the two patches we just published. So when we run `git ps ls` now we see an empty patch stack.

### Feature Complete

At this point our feature has been developed, reviewed, and published. We are done with this task and can now move on to the next task.

## It is just Git

Fundamentally [git-ps][] is just a light layer of automation of underlying Git commands. So it is **very important** to know Git. Especially in relation to this idea of iterating on your patches to a point where they are ready for review. We have started a [git-ps Guide](https://github.com/uptech/git-ps/wiki/Guide) where we enumerate various activities that someone might want to perform, and we provide examples of how they can be accomplished with Git and [git-ps][].

If we just look at [interactive rebases](https://git-scm.com/docs/git-rebase) in Git we see that there are many things you can do within an interactive rebase. For example you can squash commits, fixup commits, edit commits, reorder commits, reword commit descriptions, and many more. Also there are other Git commands that can aid in this evolution of your patches. For example [`git add -p`](https://drewdeponte.com/blog/git-add-patch-wont-split/) and [`git commit --amend`](https://git-scm.com/docs/git-commit).

Beyond that, it is worth understanding how you can use these tools to do things like [Split Commits Up](https://drewdeponte.com/blog/git-splitting-commits/). Please take the time to make sure you understand these Git commands and how to use them. It will not only improve your workflow within this methodology but with using Git in general.

And remember, this is just Git. If we ever run into situations where we need to step outside this methodology-say, to use a branch-there is nothing preventing us from doing so. In fact [git-ps][] sees any branch with a tracked upstream branch as a patch stack. So you can think of branches as separate stacks of patches.

We have found this flexibility to be useful when working with clients that don't follow the Patch Stack methodology. It enables us to still follow the practices of Patch Stack locally despite our client using a different methodology, e.g. Feature Branch development. This enables us to get at least some of the beneficial values of the Patch Stack methodology while still playing nice with others.

[git-ps]: https://github.com/uptech/git-ps

