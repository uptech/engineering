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
* [Riverpod](https://riverpod.dev)

## Navigation

* "Go Router" - [link](https://gorouter.dev)

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


## VS Code Extensions

### [Dart](https://marketplace.visualstudio.com/items?itemName=Dart-Code.dart-codes)

* Dart language support and debugger for Visual Studio Code.

### [Flutter](https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutters)

* Flutter support and debugger for Visual Studio Code.

### [Json to Dart Model](https://marketplace.visualstudio.com/items?itemName=hirantha.json-to-darts)

* Extension convert Json to Dart Model class

### [Awesome Flutter Snippets](https://marketplace.visualstudio.comitems?itemName=Nash.awesome-flutter-snippetss)

* Awesome Flutter Snippets is a collection snippets and shortcuts for commonly used

### [Flutter Intl](https://marketplace.visualstudio.com/items?itemName=localizely.flutter-intls)

* Flutter localization binding from .arb files with official Intl library

### [Error Lens](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlenss)

* Improve highlighting of errors, warnings and other language diagnostics.

### [Flutter Riverpod Snippets](https://marketplace.visualstudio.comitems?itemName=robert-brunhage.flutter-riverpod-snippetss)

* Quick and easy Flutter Riverpod snippets

## Theme Extensions

TBD
