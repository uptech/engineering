+++
title = "Flutter Resources"
description = "A brief listing of useful Flutter resources"
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 420
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

## State Management

A predictable state management library for Dart

* [Bloc State Management Library](https://bloclibrary.dev/)
* [Bloc vs Cubit](https://ppantaleon.medium.com/flutter-bloc-vs-cubit-100a0fb0efcf)

## Navigation

* "Official Flutter Navigation 1.0 + 2.0" ([link](https://flutter.dev/docs/development/ui/navigation))
* "Nav 2.0 + Deep Links" - Kevin D Moore via Ray Wenderlich ([link](https://www.raywenderlich.com/19457817-flutter-navigator-2-0-and-deep-links))
* "Simple Guide to Nav 2.0" - Lee Phillips ([link](https://medium.com/geekculture/a-simpler-guide-to-flutter-navigator-2-0-part-i-70623cedc93b))
* "Building a Simple Stack Navigator with Flutter Navigation 2.0" - John Cassidy ([link](https://johnwcassidy.medium.com/building-a-simple-stack-navigator-with-flutter-navigation-2-0-f11b55b71520))

## DI

* "Get It" - ([link](https://pub.dev/packages/get_it))

## Network

* "Dio HTTP" - ([link](https://pub.dev/packages/dio))
* "Dio HTTP Logger" - ([link](https://pub.dev/packages/pretty_dio_logger/versions/1.2.0-beta-1))
* "Flutter GraphQL" - ([link](https://pub.dev/packages/graphql_flutter))
* "Artemis GraphQL Schema Code Generator" - ([link](https://pub.dev/packages/artemis))

## Create Plugins

* "Writing a good Flutter plugin" - Mehmet Fidanboylu ([link](https://medium.com/flutter/writing-a-good-flutter-plugin-1a561b986c9c))
* "How to write a Flutter Web Plugin" aka How to Use Platform Interface for Plugins - ([link](https://medium.com/flutter/how-to-write-a-flutter-web-plugin-part-2-afdddb69ece6))

## iOS Schemes/Android Product Flavors

* "Flutter — Separating Build Environment with Multiple Firebase Environment" - Damodar Lohani ([link](https://lohanidamodar.medium.com/flutter-separating-build-environment-with-multiple-firebase-environment-92e40e26d275))
	* Video 1 - Setting up main_dev.dart and main_prod.dart environments ([link)(https://www.youtube.com/watch?v=DgGUtTUatDQ))
		* We more commonly use env.dev and env.prod for environment variables, which differs from this method. However the next two videos will work with either setup.
	* Video 2 - Setting up Flavors for Android ([link](https://www.youtube.com/watch?v=UZFIMRAWtgw))
	* Video 3 - Setting up Schemes in Xcode for iOS ([link](https://www.youtube.com/watch?v=gdqnxcV7_FY))
		* When setting up Schemes in Xcode, make sure the **Shared** box is checked on each new scheme and that the generated scheme files (ex: dev.xcscheme) are included in the commit to the repo so the next dev can build the project with the correct settings. 

## Theme Extensions

TBD
