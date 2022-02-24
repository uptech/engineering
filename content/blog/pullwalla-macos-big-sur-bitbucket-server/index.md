+++
title = "Pullwalla - Big Sur & The Future"
description = "Announcement about numerous Pullwalla releases to support Big Sur and where we are taking things in the future."
date = 2020-11-18T01:32:01-08:00
updated = 2020-11-18T01:32:01-08:00
draft = false
template = "blog/page.html"
author = "Drew De Ponte"
+++

Hello!

I wanted to clue you into some details around a few recent releases.

## macOS

Pullwalla macOS [v2.6.0](https://pullwalla.com/changelog/v2-6-0-30/) & [v2.6.1](https://pullwalla.com/changelog/v2-6-1-31/) both address the following items.

#### macOS Big Sur Support

As I am sure you already know. macOS Big Sur was officially released. With it comes a completely new style that makes macOS Apps feel a little more like iPad apps. Which [Pullwalla macOS](https://pullwalla.com) now supports.

![](https://files.pullwalla.com/pullwalla-macos-2-6-1-31-on-big-sur.png)

#### Missing Pull Requests

One of the ongoing challenges that we have had all along is the lack of documentation across the various platforms we support.  This results in JSON decoding failures on our end, in turn causing a PR or two to not show up every once and a blue moon.

We are constantly addressing these issues as we discover them to make sure you never miss a PR. These releases are no different. Combined we addressed five such issues making sure Pullwalla never misses a PR.

## iOS

Pullwalla iOS [v1.3.2](https://pullwalla.com/changelog-ios/v1-3-2-9/) & [v1.3.1](https://pullwalla.com/changelog-ios/v1-3-1-8/) were released addressing the same missing pull requests issues as above in the macOS release.

### Bridging the Gap (SwiftUI)

These Pullwalla releases on both macOS & iOS mark the beginning of a new age where we have started building cross-platform UI components using [SwiftUI](https://developer.apple.com/xcode/swiftui/).

These releases now use a shared SwiftUI View between both Pullwalla macOS & iOS for adding a Bitbucket Server Account.

This is just the beginning as all the new features & UIs we are building are being done with SwiftUI. Also any views that we have to adjust or rework are being done in SwiftUI. This will help us bring the same set of features to both Pullwalla macOS & iOS.

### Thoughts or suggestions?

As always, we’d love to hear from you.

Cheers,
The Pullwalla team

