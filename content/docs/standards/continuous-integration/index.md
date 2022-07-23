+++
title = "Continuous Integration"
description = "A living document for our experimental Continuous Integration process."
date = 2022-07-22T18:20:00+00:00
updated = 2022-05-22T18:20:00+00:00
draft = false
weight = 400
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

We are experimenting with a new variation of our workflow based on the Continous Integration Methology. It is designed to facilitate feature level engineering reviews while still enabling us to follow the Continuous Integration Methodology and operate purely on the main trunk.

## TL;DR

### Create Patch

* use [Git Patch Stack][] and follow [Git Commit Standards][] as you normal would.

### Integrating a Patches

* integrate as soon as patch is complete and passes all automated checks verifying the requirements: **buildable**, **testable**, **isolatable**, **releasble** are met.
* request review of the patch & **immediately** integrate the patch
	```
	gps rr <patch-index>
	gps int <patch-index>
	```
*Note:* The `gps rr` is done solely to create notifications in GitHub as some people use those notifications to help with the "Keeping up with the code base".

### Keeping up with the code base

* `gps fetch` or `gps pull` show the upstream patches
* scan patches to identify any that are of immediate interest
	* related to what you are working on
	* patch created by someone you are mentoring, etc.
* review immediate interest patches by going to their GitHub Commit pages and commenting on the code with questions, suggestions, etc.
* track patches you have looked at with [Sublime Merge][] commit selection state & auto refresh
* read the commits to keep up with understanding how the code base is changing. To comment, ask questions, or make suggestions open the GitHub Commit page for that commit.

*Note:* Some people use the PR notifications in GitHub to help them do this.
Ideally we would have some better tooling around this. Exploration in this is still happening.

### Complete Feature

Once all your feature's patches are integrated:

* add a git ticket summary for each of the repositories involved as a comment, use [git-ticket-summary][]
* move the user story you have been working to the "Needs Review" column.
* move onto your next task until someone reviews your feature

### Feature Review

You sould try and review a feature everytime you put a story in "Needs Review".

* pick a story in the "Needs Review" column that you want to review
* drag the story you are going to review to "In Review"
* **review the feature's functionality** in the **dev** environment to gain context and make sure it is meeting all the acceptance criteria
* **review the application architecture** involved (how this feature works through the application architecture) while noting any logic/algorithms that need further focus
* **review logic/algorithm design** in any areas of possible concern identified above
* provide your feedback on the feature's application architecture & algorithms/logic in the ticket
* if helpful you can comment on code in the commits, but make sure you still reference it in your comments in the ticket
* if there are changes that need to be made move the ticket back to "In Progress" for the developer to address the concerns
* rinse and repeat
* once the story is good move the ticket to "Ready for Release"

## References

### GitHub Commit Pages

If you aren't familiar with GitHub's Commit pages. You can access them with the following url structure.

	https://github.com/uptech/git-ps-rs/commit/da5944f6157ff29cedaf8972e7f82f0d4e7a3a8c

The SHA at the end of the URL, `da5944f6157ff29cedaf8972e7f82f0d4e7a3a8c`, can be replaced with an SHA or short SHA of a commit get the URL for that commits GitHub Commit page.

This is useful because GitHub allows you to inline comment on the within the commit similar to how you can within Pull Requests.

*Note:* Some terminals support detecting and linking to the GitHub Commit Pages from the SHAs or short SHAs in the terminal buffers, e.g. Visual Studio Code, [Alacritty][].

### Sublime Merge

If you aren't familiar with [Sublime Merge][] it is a very nice cross-platform Git GUI that detects changes to the Git repo and auto refreshes state. This makes it extremely nice as you can be working in a terminal or another app and make changes and they automatically update and reflect in [Sublime Merge][].

In the context of this process I keep it up with the repository opened and as I `gps pull` I can quickly and easily see the new patches in [Sublime Merge][] and checkout the code right there. If I find something I need to comment on I simply open that commit's GitHub Commit Page and start a discussion about the code.

[Git Patch Stack]: https://git-ps.sh
[Git Commit Standards]: /docs/standards/git-commit/
[Sublime Merge]: https://www.sublimemerge.com
[git-ticket-summary]: https://github.com/uptech/git-ticket-summary
[Alacritty]: https://github.com/alacritty/alacritty
