--- 
description: "This article covers known issues and limitations related to Visual Studio Debugger in Visual Studio Tools for Apache Cordova 2015."
title: "VS Debugger Known Issues and Limitations"
author: "jmatthiesen"
ms.prod: "visual-studio-dev14"
ms.date: "09/10/2015"
ms.author: jomatthi
--- 

# <strong>VS Debugger Known Issues and Limitations</strong>

> **Important**: We no longer maintain this article but if you’re stuck, ask us a question on [Stack using the tag ‘visual-studio-cordova'](http://stackoverflow.com/questions/tagged/visual-studio-cordova). Also, subscribe to our [developer blog](https://microsoft.github.io/vstacoblog/). We regularly post issues and workarounds.

This article covers known issues and limitations related to Visual Studio Debugger in Visual Studio Tools for Apache Cordova 2015.

## <strong>No debugger support for Windows Phone 8 (WP8)</strong>

There is currently no Visual Studio debugger support for Windows Phone 8. Developers can use the Weinre (Web Inspector Remote) project as described in this [blog post](https://msopentech.com/blog/2013/05/31/now-on-ie-and-firefox-debug-your-mobile-html5-page-remotely-with-weinre-web-inspector-remote/) from MS OpenTech as an alternative.

## <strong>Debugger does not stop at breakpoints when the app is initially starting</strong>

Due to a race condition, the VS debugger will not consistently stop at breakpoints that occur prior to the first page load in Ripple or Android emulators or devices. This can be problematic when attempting to debug code tied to page "load" or "device ready" events on the very first page of your app. However, these breakpoints will be hit after refreshing the browser (Ripple) or executing `window.location.reload()` from the JavaScript Console.

## <strong>Debugger will not start</strong>
If, after upgrading Visual Studio, you see the error message, "The debugger cannot continue running the process. Unable to start debugging.", a library may have been corrupted. Try the following workaround. Close all VS instances, then go to %appdata%\username\Local\Microsoft\Phone Tools, and rename the CoreCon folder to another name such as CoreCon2. Then try restarting your app in VS.

## <strong>DOM explorer shows Ripple HTML in addition to app HTML</strong>

When debugging an application deployed to the Ripple emulator, the JavaScript Console's default execution target is the Ripple top-level, instead of the application frame. To switch to the execution target of the user’s application, change the execution target of the JavaScript Console to the "frame: &lt;application html page&gt;" target using the target selector in the upper right of the JavaScript Console window.

## <strong>No debugging Release versions of Android, iOS</strong>

You cannot use the VS debugger for apps deployed to Android emulators or devices that are built in the Release Solution Configurations (by design) because they are signed. JavaScript console output is, however, captured in the Output window.

## When using the VS Debugger with Android 4.4+ emulators, devices, or Apache Ripple

* **VS debugger detaches when using Chrome Dev Tools:** If you start up Chrome Dev Tools in Ripple when debugging from VS, Ripple will shut down. To use Chrome Dev Tools, start without debugging (Ctrl+F5).

* **Limitations:**
  * Not all JavaScript Console APIs are available.
  * DOM Explorer Events and Changes panes are not available.
  * If an HTML file only contains script that ran and was immediately unloaded, the HTML will never appear in the Solution Explorer.
  * Only script blocks that contain code that can still run will be visible.
  * An HTML file will appear only under Script Documents in Solution Explorer if the HTML file contains code that can still run in the engine.

## When using the VS Debugger with Android < 4.4 emulators or devices

* **No debugger support for Android < 4.4 w/jsHybugger:** You cannot use the VS Debugger for apps deployed to emulators or devices running Android versions prior to 4.4 without the use of a 3rd party plugin like jsHybugger. JavaScript console output is, however, captured in the Output window.

* **Unable to start program error:** While debugging to devices with Android versions prior to 4.4, an error popup may display the error “Unable to start program” citing “adb.exe” as the cause. The app should still load and work on your device, without debugger support.

## When using the jsHybugger plugin for debugging Android < 4.4

**Limitations:** The following limitations exist when using the VS debugger with jsHybugger:

  * There is no support for Source Maps.
  * While using the DOM explorer with jsHybugger, there is no support for:
    * Deleting CSS properties.
    * Selecting DOM elements by clicking on them.
    * Add/edit/delete of element attributes.
    * Add/edit of CSS rules.
    * Forcing Hover and Visited Pseudo-class states.
    * Undo/redo.
    * Edit as HTML.
    * Shorthand property display is generally not supported.

**JavaScript console read only:** While using the JavaScript Console with the jsHybugger, the JS Console works as output only – messages are logged but we do not execute commands.  We also do not support source locations of messages, clearing messages on navigation, and expanding objects and properties of the logged messages.
