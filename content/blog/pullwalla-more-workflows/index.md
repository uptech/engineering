+++
title = "Pullwalla - More Workflows"
date = 2023-11-28T01:32:01-08:00
updated = 2023-11-28T01:32:01-08:00
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/logo-pullwalla-macos-big-sur.png"
+++

Hello!

We just released v3.7.0 (51) of [Pullwalla](https://pullwalla.com) to both macOS and iOS platforms.

## Quick Filters

One of Pullwalla's primary goals has been to help people quickly **discover** the pull requests they are interested in. Quick Filters continue to be a key mechanism supporting this. 

### More Workflows

One of the things we have done in this release is support additional workflows.

The workflow we supported historically is a workflow where developers take a step back from code every so often, looked at the previously *Unclaimed*, pull requests to find a pull request review, and then "claim" it by assigning themselves to it.

This workflow is fine and works for some. But it doesn't work for everyone. Some people instead take the approach of requiring that every pull request when created has to have explicit requests for review, or requires that assignees be used to mean something different. Others have a workflow that is some sort of hybrid between the two approaches.

Therefore, we introduced two new Quick Filters.

- **Undecided** - shows pull requests that have not been approved or had changes requested yet.
- **Needs my Decision** - shows pull requests that either you are an assignee of or that have a request for your review and have not been approved or had changes requested by you.

The **Needs my Decision** quick filter facilitates workflows where PRs are created and given an assignee or a request for review is explicitly made. While the **Undecided** quick filter supports workflows where developers pick their own pull requests to review. You can see this in the screenshot below.

![](https://files.pullwalla.com/v3.7.0-51-quick-filters-transparent-wallpaper@2x.png)

### Clarification

Beyond the addition of the **Undecided** and **Needs my Decision** quick filters. We also renamed the other quick filters to try and make them more accurate and clear. Previously Pullwalla's Quick Filters looked as follows.

![](https://files.pullwalla.com/v3.6.2-50-quick-filters-transparent-wallpaper@2x.png)

The mapping of the name changes are as follows.

- **Created** is now **My PRs** as it shows pull requests that you have created.
- **Assigned** is now **My Assignments** as it shows pull requests that you are an assignee of.
- **Mentioned** is now **My Mentions** as it shows pull requests that you are mentioned in.
- **Review Requests** is now **Request for my Review** as it shows pull requests where someone has requested your review.
- **Participated** is now **Participated In** as it shows pull requests that you have participated in.
- **Unclaimed** is now **Unassigned** as it shows you pull requests that have no assignees.
- **Claimed** is now **Assigned** as it shows you pull requests that have one or more assignees.

We hope these improvements to Quick Filters will have a big impact on peoples day to day interactions with Pullwalla. If you think we are missing something or have any feedback in general please don't hesitate to reach out at [pullwalla@uptechstudio.com](mailto:pullwalla@uptechstudio.com).

### Thoughts or suggestions?

As always, we’d love to hear from you.

**Need help with an App (mobile, web, desktop, etc.)?** [Uptech Studio](https://uptechstudio.com), the creators of Pullwalla, are here to help. Just reach out to [info@upechstudio.com](mailto:info@uptechstudio.com), and we are happy to explore how we might work together.

Cheers,
The Pullwalla Team
