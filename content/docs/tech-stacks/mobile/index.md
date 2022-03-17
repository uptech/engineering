+++
title = "Mobile"
description = "Our preferred mobile tech stacks"
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 10
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

We effectively have two main camps that we support & use for Mobile development.

* cross-platform
* native

## Cross-Platform

Lots of clients for good reason don't want to have to invest in two teams building effectively the same application in two different tech stacks at the same time. So instead they opt for a cross-platform solution. There are really two primary competitors in this space, [ReactNative][] and [Flutter][].

We have selected [Flutter][] as our cross-platform tech stack that we work in because it has consistently been growing and even outgrown [ReactNative][] in terms of community and support.

## Native

We of course do native development as well. This is crucial even for some cross-platform apps as often times you have to build a bridging layer to facilitate the cross-platform apps interfacing with the native platform. But if we are working with a client that values that native feel and performance enough that they are willing to pay to build native [Android][] and [iOS][] applications we do that as well.

In the [iOS][] space we use the latest and greatest tools and technologies e.g. [Swift][], [SwiftUI][], [Combine][], etc.

In the [Android][] space we use the latest and greatest tools as well, e.g. [Kotlin][], [Jetpack][], etc.

[ReactNative]: https://reactnative.dev 
[Flutter]: https://flutter.dev/
[Android]: https://www.android.com
[iOS]: https://www.apple.com/ios/
[Kotlin]: https://kotlinlang.org
[Jetpack]: https://developer.android.com/jetpack
[Swift]: https://www.swift.org
[SwiftUI]: https://developer.apple.com/xcode/swiftui/
[Combine]: https://developer.apple.com/documentation/combine
