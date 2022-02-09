+++
title = "Our Tools"
date = 2022-02-09 # or weight 
description = "A breakdown of our custom tools"
insert_anchor_links = "right"

[taxonomies] #all taxonomies is optional
tags = ["tools"]
authors = ["Drew De Ponte"]
+++

Here at UpTech we like to focus on doing the best work we can. This involves a lot of personal growth over time to become true craftsman at our trade. To aid in this process we often build our own tooling or hone existing tools and libraries. Below are some quick summaries of the various tools we have built and their respective impetus.

## Pullwalla

Given our heavy focus on quality and craftsmanship. We focus heavily on a peer review process to aid with personal growth, implementing long term maintainable application architectures & software. However, in our heavy use of pull requests to facilitate our peer review process we ran into the following issues.

* existing SCM systems lack efficient mechanisms to see pull requests across multiple Git repositories, let alone multiple SCM platforms & accounts
* existing SCM systems don’t support an open team, self selective peer review process to better facilitate cross-polination and accelerated silo elimination within the dev team
* existing SCM systems have poor or in some cases non-existent mobile experiences making it extremely difficult to review, comment on, or respond to any pull requests when on the growth

[Pullwalla][] is a product we have built specifically to address these issues. We currently sell it on the Apple App Store and we provide macOS, iOS, & iPadOS versions of the application.

If you are an employee of UpTech Works, LLC reach out to drew at uptechworks.com to request a promo code to unlock the full version of Pullwalla on all the Apple platforms.

To redeem your Promo Code you need to do so via the Apple App Store application. Click your profile image in the lower left corner. Note: You should be using your personal Apple ID on your laptop. Then click the “Redeem Gift Card” button in the upper right corner. Then paste your promo code into the text box and click the “Redeem” button. Once you have redeemed the Promo Code in the App Store you can launch Pullwalla and go to the Settings - Pro tab and click the “Restore Purchases” button. This should unlock the Pro version of Pullwalla.

## Git Patch Stack

Over the years we have determined that there are some pretty serious issues/drawbacks with how people classicly think of pull requests and the concept of **feature branches**, which seemingly & incorrectly have be intangibly linked for some time. Specifically, we have identified that the use of the feature branch concept tends to:

* promote non-logical commits, prohibiting certain Git functionalities (bisect, etc.) from working & making **valuable** peer review impossible
* promote extremely large pull requests, making **valuable** peer reviews impossible & time consuming
* promote lack of care around structuring commits which ofter results in people leaving WIP commits in pull requests
* have extremely heavy overhead in managing feature branches that depend on other feature branches
* often make the Git tree (history) extremely difficult to read & reason about

[Git Patch Stack][] is an open source extension we built to Git that addresses all of the above issues by removing the need for the concept of feature branches and introducing a concept and local development workflow we call, [Git Patch Stack][]. You can install it by going to the GitHub repository and following the instructions in the README.

For more details on how to use Git Patch Stack you can checkout out our blog post [How we should be using Git][]. This provides the background and conceptual thinking around Git Patch Stack while also providing an example. Beyond that you can checkout the [Git Patch Stack Guide][]. It provides examples of common tasks you would take and details around them.

If at any point you have any questions or need help with Git Patch Stack or just Git in general. You can always reach out to drew at uptechworks dot com and he will help you.

## Git Changelog

Over all our years of experience we have found that maintaining a CHANGELOG file is extremely valuable and definitely worth the effort. However, this was always a point of friction as doing so within a Git repository would result in conflicts with every change simply because of the changes to the CHANGELOG file. [Git Changelog][] is an open source Git extension that we built to specifically address this issue and it has become our standard way of managing CHANGELOG files in projects.

## TeamSmash

Given that UpTech is remote company. We decided it was important to still feel connected & part of a team. So we built a remote connectivity app called TeamSmash, [https://teamsmashapp.com](https://teamsmashapp.com), to address this issue. It is a free app in the macOS App Store so download it, get setup, and feel like part of the team.

[Pullwalla]: https://pullwalla.com
[Git Patch Stack]: https://github.com/uptech/git-ps 
[How we should be using Git]: https://upte.ch/blog/how-we-should-be-using-git/
[Git Patch Stack Guide]: https://github.com/uptech/git-ps/wiki/Guide
[Git Changelog]: https://github.com/uptech/git-cl 
