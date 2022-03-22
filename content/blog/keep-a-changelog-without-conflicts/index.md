+++
title = "Keep a Changelog without Conflicts"
date = 2020-05-11T08:30:01-08:00
updated = 2020-05-11T08:30:01-08:00
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/logo-git.png"
+++

For years, like many others, we have maintained `CHANGELOG.md` files inside our
[Git][] repositories for our apps and libraries. We have followed the standards
outlined at [keepachangelog.com][] and we have appreciated the benefits it has
provided us for preparing releases and for maintaining a clear record of
changes included in each release while still being targeted at the appropriate
audience.

However, these benefits didn't come without drawbacks. The biggest being that
while working on a repo with multiple people or even developing locally using a
patch stack workflow you continually run into [Git][] conflicts solely with the
`CHANGELOG.md` file. These conflicts aren't meaningful in anyway. They are just
an impedance to our development flow and process, both as a team and as
individual contributors.

Well a few weeks ago [Anthony Castelli][] and [I][] decided it was time to
remove this impedance not only for our local workflows but for the development
workflow of the team. We started out by analyzing the characteristics of the
`CHANGELOG.md` approach that we valued and the characteristics that were
causing the issues. The biggest turned out to be one and the same, the tight
coupling of the source code change to the `CHANGELOG.md` entry. This tight
coupling is what made it easy to prep releases of libs/apps because the
developer that made the change, the person with context, also created the
associated `CHANGELOG.md` entry. However, this was also the practice that was
triggering the useless conflicts in the `CHANGELOG.md`.

So we hashed this out and eventually came to the realization
that we could have the tight coupling of the Changelog entry and the code while
avoiding the conflicts all together. We just had to lift the Changelog entry up 
to the [Git][] commit message. After contemplating implementation approaches
and realizing at the core it would be similar to [Git Patch Stack][] we
immediately jumped into building a proof of concept [Git][] command line tool
extension to facilitate building a Changelog from entries in the [Git][]
commits.

Over the last few weeks we have been using it in our workflows and iterating on
it to make sure it meets everyones needs. This work has resulted in our latest
open source project, [Git Changelog][]. Below is a ~15 min video where I
introduce the concepts, values, the `git-cl` tool, as well as it's best
practices.

<div style="text-align: center;">
<iframe width="560" height="315" style="margin: 0 auto;" src="https://www.youtube.com/embed/g7xqWKmIUKI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

[Git]: https://git-scm.com
[keepachangelog.com]: https://keepachangelog.com
[Anthony Castelli]: http://anthonycastelli.me 
[I]: https://drewdeponte.com
[Git Patch Stack]: https://github.com/uptech/git-ps
[Git Changelog]: https://github.com/uptech/git-cl
