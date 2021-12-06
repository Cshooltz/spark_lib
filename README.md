# Spark Lib - Flutter
### Description
Spark Lib is a personal shared library for use with my Flutter projects. My goal for the library is to build into it most of the common application components I regularly use in my projects. Currently it contains:
- A page-based navigator system built on top of Flutter's Navigator, 
- the beginnings of custom implementations for the standard Material in-app notifications (i.e. Snackbar, Banner, Dialog),
- app initialization boilerplate, 
- platform-independent filesystem locations (based on path_provider),
- custom desktop window titlebar and decorations (via Bitsdojo package),
- and a smattering of other minor things.

It is somewhat haphazard and I am actively developing it alongside my application projects. Expect it to improve over time.

If you're new to Dart, the [lib](lib) directory contains all the code for the project. The library is split into folders based on categories.

### How to Use

Much of Spark Lib contains standalone reusable components that can be simply imported and used as needed. The main parts of the library that require specific setup are the application scaffold and custom window titlebar, which are detailed in the following subsections.

#### Rapid Application Scaffold

The main application scaffold components of Spark Lib reside in the [lib/app](lib/app) and [lib/navigation](lib/navigation) directories. [lib/app](lib/app) contains the `AppSystemManager` widget and `SparkApp` class, which work together to form the root of the Flutter application. For a very fast startup, you may simply create an instance of SparkApp and pass the result of `SparkApp.build()` to Flutter's `runApp()` function.

[lib/navigation/spark_nav.dart](lib/navigation/spark_nav.dart) exports the `SparkPage` class and static `AppNavigator` class. In order to use `AppNavigator` to manage navigation within your app, every dedicated screen (not intended to be an overlay) must be wrapped in a `SparkPage` widget in order to work with the custom `onWillPop()` methods used by `AppNavigator`.

Once you make the pages for your app, you can make navigation calls to `AppNavigator` instead of `Navigator.of(context)` for global app navigation. To switch to a new page, simply pass a `SparkPage` wrapped widget to `AppNavigator.navigateTo()`.

#### Custom Window Titlebar for Desktop
I have also built in code that takes care of the boilerplate associated with adding a custom titlebar and window behavior to desktop applications. The required code is located under [lib/custom_window](lib/custom_window). The file [bitsdojo_boilerplate.dart](lib/custom_window/bitsdojo_boilerplate.dart) contains the `initializeBitsdojo()` function, which wraps the setup needed for the bitsdojo_window package to work and must be called after `runApp()` is called in your application's `main()` function.

The file [window_appbar.dart](lib/custom_window/window_appbar.dart) contains the code to create a modified `AppBar` (via the static `AppBar.build()` function) for use with the `Scaffold` widget in Flutter. My implementation makes a standard Material app bar, but makes the app bar act as the titlebar for the app, meaning it can be dragged to move the window and includes OS standard minimize, maximize, and close buttons. If the app is executed on mobile, the app bar will be a plain Flutter Material `AppBar` with no modifications.

**Note:** In order to use the bitsdojo_window package to create a custom titlebar, you must modify the runner code for each platform per the [bitsdojo_window package instructions](https://pub.dev/packages/bitsdojo_window).
