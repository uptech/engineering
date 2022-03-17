+++
title = "Pull Request"
description = "Explanation of our Pull Request requirements & how we get there"
date = 2022-03-01T18:20:00+00:00
updated = 2022-03-01T18:20:00+00:00
draft = false
weight = 420
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

We **require** that Pull Requests have the following characteristics.

* small
* logical
* buildable
* testable
* releasable

### Small

It is crucial that pull requests are **small**. The quality of peer review exponentially decreases as the size of the pull request increases. If you are going to have PRs in the hundreds or thousands of lines of change range you may as well just not do peer review. Here at [Uptech Studio][] we believe in producing the highest quality code that we can and therefore we **must** have **small** pull requests.

If you are interested in how we got to small pull requests, checkout our post [Journey to Small Pull Requests](/blog/journey-to-small-pull-requests).

### Logical

It is also important that pull requests are **logical**. This means that the bounds of the content are well ordered and have clear & sound reasoning.

You might say well a *feature* is **logical**. Well, you would be correct. However, having a pull request contain an entire feature is in contradiction with the requirement of a pull request being small. Therefore we need to find some other logical bounding for the change. Generally application architecture concepts are a great aid for this. For example you could have a pull request that adds a method to a service to allow consumers to get a collection of items which would be logically bound around the change to that service. It could also be that you are making a cross-cutting change that is still logical. For example you could have a pull lequest that changes code from using strings to symbols which is spread across multiple application architecture concepts but is still **logical**.

Having a pull request be logical is crucial not only for understanding the evolution of the code base over time but is crucial to support functionality in [Git][] like `bisect` and `revert`.

## Buildable

Pull requests also must be **buildable**. Meaning that the state of the code in the pull request has to be in such a place that the library/framework/application/suite can be built without error. This is crucial again to support functionality in [Git][] like `bisect` and `revert` but is also import to not impede the development process of the team.

## Testable

A pull request being **testable** means that the change has automated tests covering it and that the test suite is able to be run with those tests included. In cases where code isn't able to be tested with automated tests, maybe some UI change, then the changes have to be **manually testable** and the pull request description should include enough information to educate someone on how to manually test it.

This is crucial, especially when making smaller pull requests because it doesn't make sense to pull down a pull request and manually test changes to an internal application architecture concept. So instead of getting our confidence from manual testing we get it through making sure that we have well written automated test coverage and that they are passing.

## Releasable

We also require a pull request to be **releasable**. This doesn't mean that you would idealisticly want to cut a release with just this one pull request. But it means that if push came to shove and you needed to for some reason the application/library would be in such a state where it could be released.

This generally is a pretty easy requirement to meet as you can have code in your code base that is **buildable** & **testable** that isn't used, and that is totally fine. The complexity with this requirement comes when you are dealing with changes that are visible to the consumer of the library/application. That is where you have to then ask yourself if I make this change to the UI if push came to shove would this change be **releasable**?

Remember it isn't that if it was released it would be the ideal release. It is that it would be possible to release it.

[Uptech Studio]: https://uptechstudio.com
[Git]: https://git-scm.com
