+++
title = "Pra - The App Store Audible"
date = 2019-02-22T09:32:01-08:00
updated = 2019-02-22T09:32:01-08:00
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/logo-pra.png"
+++

It is Sunday, Feb 17th, 2019 at 10:58 pm and I have just submitted the **Minimal Free Version** of [Pra][] to the Apple App Store for review. I post in our team Slack channel that I've submitted and I cross my fingers as we wait for Apple to respond.

## Minimal Free Version
It is probably worth explaining that our **Minimal Free Version** is pretty small in terms of functionality. It lets you do the following things:

- **Manage an Account** - Add or remove your GitHub account.
- **See a Feed of Pull Requests** - See a continuously updating feed of pull requests you potentially want to review, aggregated across all repos you have access to, with the necessary data to quickly assess if you want to review one of them.
- **View a Pull Request** - As you're looking through open requests, if you see one you want to review, you can quickly view the pull request details in your default browser.
- **Ignore Repositories** - Select a pull request in the feed and ignore/hide the repository it's in from the feed of pull requests. Extremely helpful in reducing noise in your pull request feed.

We thought that the tool was already valuable in this state and we just wanted to get it in the hands of users to validate our assumptions. Would other people find this minimal state of the product valuable? If they don’t, it would be extremely useful to learn why not. If they do find it valuable, that is reassurance we are on the right path. Either way, we're interested in feedback that will help us decide which features to focus on and when, including what should be in the paid version and what should be in the free version.

## The App Store Response
It is now Feb 18, 2019 at 7:22 PM and we just received a rejection response from Apple.

> **Guideline 4.2.2 - Design** 
>   
> The experience that your app provides is not sufficiently different from browsing a content aggregator web site.  
>   
> **Next Steps** 
>   
> While your app may facilitate access to content from a range of web sites, even when including native macOS features, the experience it provides is not significantly different from using Safari. Such apps do not include enough native macOS functionality to be appropriate for the App Store.  

## Headspace
To be honest, getting this response was frustrating and a little disheartening. Part of why I feel that way I think is because Apple's response is so generic and unclear. What do they consider native macOS functionality? The app is 100% written in Swift, doesn’t use any web views, and integrates with macOS Keychain and UIKit. There are also plenty of apps on the App Store that don't seem to include any more native macOS functionality than [Pra][], even in its current, minimal state. Somehow those apps were allowed on the App Store. So a few of us (mostly me) start venting a bit about this in Slack, while we slowly start to calm down.

Once I calmed down, we started throwing out ideas about what "enough native macOS functionality" could mean to Apple. We look at RSS Feed Reader apps because they are "content aggregators" and there are a ton of them in the App Store. Most of them provide a viewer interface for the posts, so we think maybe we need to add a _pull request viewer_ in the app rather than just open the PR in the default browser. This is already in our idea backlog, so it's not out of left field.

Adam suggested trying to find one or two features that would take minimal effort to build, but be unique to the desktop, to see if Apple would then accept the app as having "enough" native functionality. He threw out a few ideas:
- Some sort of notifications
- Badge on dock icon
- Banners/alerts
- Menu bar icon

Another thought I had rolling around in my head was how much of the rejection was a side effect of our App Store copy not explaining the true value of [Pra][]?

I was having a hard time homing in on exactly what we should do. So I went back to why we were trying to launch the app in the first place: to get crucial feedback and find out if other people find it valuable in its current state. At this stage, we're more interested in getting an idea of whether or not we are on the right track and gathering some feedback than we are interested in wide distribution.

## The Audible
At this point, I started thinking maybe we should just launch via direct download from our website rather than via the App Store. This would allow us to get versions out quickly and gather feedback while we continue the review process with Apple.

So, we hashed it out as a team in Slack and decided to put the download on our site (and start to gather feedback and iterate on the **Minimal Free Version**) while we continue to work with Apple to get clarification and get the app ready to resubmit to the App Store.

## The Non-App Store Launch
Feb 21st, 2019 at 1:39 PM we launched a landing page with the direct download button to get the app, and shared it with small group of colleagues in another Slack team.

### Sam’s Feedback

That afternoon I got feedback from a friend named Sam now using it and asking the following questions:
1. **How do I add more than one account?** - We realized this was an indicator that our communication via Tooltip on the add account button was too subtle to properly communicate that the free version only supports one account. Also, it made us think we should probably have a mechanism at that point to allow people to submit their email address to join the mailing list to find out about releases and when multi-account support is added.
2. **How do I connect to my Enterprise GitHub account?** - This is a feature we talked about being part of the paid features previously as well. So, we took this as reassurance that there is at least some need for this feature.

### Nico’s Feedback

Later that evening I got feedback from another friend asking about what PRs are included in the feed. He wasn’t seeing a PR that he knew he could see in the _Created at_ filter on GitHub. He thought it was because he is an External Collaborator on the repository, and it turns out he was right that that functionality was missing. So, I quickly made a ticket to resolve the bug and started working on a fix. I shipped the fix to Nico so he could test it and find out if it resolved his issue.

Unfortunately, the new version still didn’t show the PR Nico was missing, but it did add the missing functionality of showing pull requests from repositories that you are an external collaborator of. Digging a little deeper with Nico, we eventually figured out that his missing PR was from a fork of a repository he no longer had access to.

He thought it might not be worth the effort to fix (maybe this is just an edge case?). I responded that we probably wouldn't add this to the main feed view (because the main purpose of the feed is to show pull requests you might be interested in reviewing, not necessarily all the pull requests you can access). But, I also let him know that we were contemplating adding other feed views via quick filters, including a “Created” filter which would show this pull request. This would support a different user goal of checking in on the status of their own PRs, and if there is anything they can do to move them forward. Our conversation and interaction gave me reassurance that what we are thinking in terms of features, contexts/modes, etc. were right on track at least with Nico.

### The High

Both of these interactions gave us a ton of value and amped me up.  This completely made up for the feelings I had around the App Store rejection.

## The Second App Store Response
Back to Apple &mdash; we decided to try and get clarification. So, Matt and Adam put together a response to Apple.

> Is there a way that we can get clarification on this rejection? Our app provides a significantly better experience than Safari, because it stores your GitHub credentials so that you can have a persistent and live-updating view of pull requests in your GitHub account on your desktop, without having to open a browser and log into your account.   
>   
> This view of pull requests isn’t available anywhere on the web today and having it persistently available via a desktop app provides unique value that you couldn’t create with just a web site. To do this in Safari would require going into each team in Github and going to each repository for each team to see what Pull Requests are open. This is a massive headache for a developer that is working in multiple teams with multiple repos per team.  
>   
> Happy to get on a call with you guys and walk you through how it works and what our roadmap is for enhancements to the desktop experience.  

Apple came back with the following response:

> **4.2 Design: Minimum Functionality (macOS)** 
>
> Hello,  
>   
> Thank you for your response. It would be appropriate to add native macOS functionality to be appropriate for the App Store.  
>   
> Regards,  
>   
> App Store Review  

## What’s Next?
Well, Apple's response didn't give us much to go on. So, we just need to put some thought into what “macOS Features” we can easily add (and that make sense), then submit to the App Store again.

Meanwhile, we continue to tell more people about the **Minimum Free Version,** collect feedback, and iterate along. From this initial round of feedback, we've identified a few tasks:
- add pull requests for External Collaborator repositories to the feed
- add support for joining a mailing list in the Account Preferences
- add processing of pagination to GitHub API requests

From the App Store rejection we're planning to:
- review the App Store messaging and see if we can communicate the value better
- figure out what native macOS features we think make sense to add in order to get it approved for the App Store

Overall, I feel very confident that the decision to release the app outside of the App Store this early in its lifespan was one of the best things we could have done. The feedback is invaluable and each iteration we make improves the app. In retrospect this really cost about a day's worth of work to research (and implement) the proper way to package and release the app outside of the store. In the scheme of things, a relatively small price to pay for the feedback we obtained in the first round.


[Pra]: https://pullwalla.com
