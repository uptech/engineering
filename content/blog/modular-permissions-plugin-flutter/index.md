+++
title = "Modularizing Native Permissions for Flutter."
description = "Explicitly handling permission requests for Flutter applications."
date = 2020-10-29T11:30:01-08:00
updated = 2020-10-29T11:30:01-08:00
draft = false
template = "blog/page.html"
author = "Jon Holtan"
+++

On mobile (Android, iOS), developers have to ask the user to access certain runtime features. These permissions typically include the user's location, the user's contacts, or a plethora of others. Generally, when we think of cross-platform, we think of one size fits all plugins. This approach is anticipated in the Flutter plugin ecosystem. However, when it comes to permissions, this idea breaks down. 

The existing [permission_handler](https://pub.dev/packages/permission_handler) includes all of the logic to request multiple permissions into one package and then provides a set of PODFILE macros for iOS that the developer must use to remove unused permissions. While this approach works, it goes against Flutter's approach to development. If a developer is new to development in general, this configuration step may overwhelm them and turn them away. If a developer forgets to do this extra step, Apple will reject the iOS app for unused permissions. It is much safer and straight forward to be explicit with regards to permissions. Developers should include what they need and not have to worry about extra configurations, which is why we developed [Modular Permissions Plugin](https://pub.dev/packages/modular_permissions/versions/0.1.0) to give developers a clean way to request permissions in their applications.

## Flutter 
[Flutter](https://flutter.dev) is Google's UI toolkit for building beautiful, natively compiled [mobile](https://flutter.dev/docs), [web](https://flutter.dev/web), and [desktop](https://flutter.dev/desktop) applications from a single [Dart](https://dart.dev) codebase. Flutter is a production-ready solution for iOS & Android, whereas Web, Mac, Windows, and Linux are in alpha or beta stages. Flutter is designed to abstract away the native platforms' details and empowers developers with a solution to deploy to multiple platforms with ease.

Flutter can hook into native platforms to extend functionality beyond Dart. This ability allows developers to create powerful applications with relatively few limits. Connecting to native platforms is incredibly powerful when it comes to handling permissions. 

## Privacy

Privacy is important to users, and they treat permissions as an entry to sections of their private life. This trust used to be quite loose on the Android system and still is in some parts. For example, when users installed their newly downloaded app in the past for Android, they had to accept all permissions upfront. This acceptance approach was great in theory, but it led to applications such as a Flashlight app requesting your contacts, location, and storage. Crazy right? Thankfully, this all or nothing approach is no longer the case, and Android developers must request access from the user at runtime. A system that Apple has always pushed on iOS & macOS. 

## Modular Permissions Plugin

The Modular Permissions Plugin provides a cross-platform (Android, iOS) API to request permissions, check the permission's status. Developers can also programmatically open the device's app settings so users can manually grant permissions as needed. This plugin's design is a generic Dart class that knows how to talk to each specific plugin. These particular plugins focus on the logic required to request permissions. Currently, the plugin only supports the location permissions, but we will include new permissions over time. Feel free to open a [Github issue](https://github.com/uptech/modular_app_permissions/issues) with your permission requests. This design ensures that developers only have the permissions needed for their application. This approach gives developers confidence that Apple or Google will not reject the application due to unwanted permissions.

## Getting started

First and foremost, you will need to include the main plugin in your `pubspec.yaml` file.

```yaml
modular_permissions: 0.1.0
```

You need to request the user's location, so you include the modular location permission sub plugin.

```yaml
modular_location_permission: 0.1.0
```

Next, run a `flutter pub get` to fetch the new packages. Now you are ready to start requesting permissions.  

## Checking a permission's status

To check the location permission status, you call the static `checkPermissionStatus`, and you pass in a Location When In Use or Location Always permission request. Android will always request the `Access Fine Location` whereas, on iOS, you can specify which location type you want to use. To do this, await the Future and inspect the returned information.

```dart
final info = await ModularPermissions.checkPermissionStatus(LocationWhenInUsePermissionRequest());
```

If the location permission is not accessible, you can request access from the user by simply calling `requestPermission` and passing in a permission request. This function will trigger logic in the native platform, which asks the user for permission. The user can either accept or deny this request. As above, await the Future and inspect the returned information.

```dart
final info = await ModularPermissions.requestPermission(LocationWhenInUsePermissionRequest());
```

Suppose the user has denied the location permission request. You can display information to the user and ask them to accept the permission from app settings manually. To do this, you can call `openAppSettings`, and then the user can grant permission.

```dart
await ModularPermissions.openAppSettings(LocationWhenInUsePermissionRequest());
```

## Conclusion

We at UpTech are always striving to improve the development ecosystem for ourselves and current or future developers, which is why we are actively creating new plugins for Flutter and other platforms. We hope that Modular Permissions gives developers confidence that they can develop polished experiences for their users. Do you enjoy working on solutions to empower others? [Reach out](https://upte.ch/#contact) today to start a conversation. We’d love to hear from you.
