<properties pageTitle="Android tips and workarounds"
  description="This is an article on bower tutorial"
  services=""
  documentationCenter=""
  authors="kirupa" />
  <tags ms.technology="cordova" ms.prod="visual-studio-dev14"
     ms.service="na"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="mobile-multiple"
     ms.workload="na"
     ms.date="09/10/2015"
     ms.author="kirupac"/>

#Resolve build and deployment errors

This document covers suggestions for resolving build and deployments errors that may occur when you are building your Cordova app.

<a name="firsttime"></a>
##Resolving build and deployment errors when you build for the first time

Try these steps.

1. Wait for the build to complete.

    When building for the first time, Visual Studio downloads and installs Cordova dependencies. The first build can take considerably longer than normal to complete.

    Similarly, when you first start an emulator, the emulator may take considerable time to load (leave the emulator open between app restarts to avoid this issue).
2. Check your Visual Studio notifications (upper right) to see if you need any updates to Visual Studio or to Visual Studio Tools for Apache Cordova. Install any updates and retry.

    ![Visual Studio notifications](media/resolving-build-errors/vs-notifications.png)

3. In Visual Studio, run the Dependency Checker by choosing **Tools**, **Options**, **Tools for Apache Cordova**, and selecting **Run Dependency Checker**. Investigate any resulting messages.
4. Check the Output window and the Error List window for build error information and guidance.

    ![Visual Studio notifications](media/resolving-build-errors/output-window.png)

    If you're not sure what to do next, see the next section on resolving general build and deployment errors.

<a name="general"></a>
##Resolving general build and deployment errors

Try these steps if you have trouble deploying to emulators or devices.

1. Running on a device? Make sure that your device is enabled for development. Instructions to do this are different for each platform.
    * For Android, see [Android](#android). For iOS, see [iOS guide](../getting-started/ios-guide.md). (iOS devices need a provisioning profile.) For Windows Phone, see [this article](../develop-apps/run-app-windows-phone.md). For Windows 10, Visual Studio will prompt you to enable the device.
2. If you are running on a device and the app is already installed, uninstall the app and try again.
4. Delete the project's platforms/*platform* folder (such as platforms/android) and the plugins folder from the project and try again (commands like `cordova platforms remove android` also remove the platform).

    ![The platforms and plugins folders](media/resolving-build-errors/platforms-folder.png)

    If the issue only occurs with a particular plugin, you may have an incompatibility with the plugin or the plugin version. See these [tips](general/tips-and-workarounds-general-readme.md).

3. If you haven't done it already, [check your update notifications and run the dependency checker](#firsttime).
4. If you are not using a new blank project already, create an empty project using the Blank App template (you can find it under **JavaScript**, **Apache Cordova Apps** when creating a new project) and see if you get the same error when you build.
    If the Blank App template runs fine, the issue may be specific to your project.
5. Try to run your app against a different platform or emulator and see if that is successful.

    Here are a few suggested platforms for testing:
    * Try running against Android and select **Ripple - Nexus (Galaxy)** (Chrome required)
    * Open the configuration designer (config.xml), the **Windows** tab, select your OS version, and then try running against **Windows-x64** or **Windows-x86** and select **Local Machine**.

    If the issue is only on the Android platform, see [Android](#android). If the issue is only on iOS, see the troubleshooting tips in the [iOS setup guide](../getting-started/ios-guide.md). If the issue is only on Windows, see [Windows](#windows).

6. If you see a message that you are unable to start debugging, see [this workaround](#debugging).
7. Try [clearing the cache](https://taco.visualstudio.com/en-us/docs/configure-vs-tools-apache-cordova/#vstac) from **Tools**, **Options**, **Tools for Apache Cordova** and re-installing vs-tac.

<a name="android"></a>
##Troubleshooting Android build and deployment issues

Try these steps if you have trouble building and deploying to Android emulators or devices. In some cases, you may see an error about a failure to install the APK on the device or a failure to run the Android Debug Bridge (ADB.exe).

>**Note**: For instructions to run for the first time on specific emulators or an Android device, see [Run your app on Android](../develop-apps/run-app-apache.md).

1. If you are running on a device, make sure that **Developer Mode** is enabled on the device. Try the instructions [here](http://www.greenbot.com/article/2457986/how-to-enable-developer-options-on-your-android-phone-or-tablet.html) to enable developer mode.

    After you enable developer mode, go to *Developer options* on the device and enable Android debugging (USB Debugging).
    In USB settings, also make sure you are connecting as a *Media device*.
2. If you are running on a device and the app is already installed, uninstall the app and try again.
4. Verify that the Android SDK can connect to your device or emulator. To do this, take these steps.

    * Start an emulator or connect a device.
    * Open a command line and go to the folder where abd.exe is installed. For example, this might be C:\Program Files (x86)\Android\android-sdk\platform-tools\adb.exe
    * Type the command `adb devices` and you should see a connected device.
    ![See the connected devices](media/resolving-build-errors/adb-devices.png)

    If you don't see the device, restart your device or emulator and use a different USB port. (Sometimes, front USB ports are low power ports. Avoid using a USB extension cable.) Recheck using `adb devices`.

    For additional ADB commands, see [this article](http://www.androidcentral.com/android-201-10-basic-terminal-commands-you-should-know).

5. Make sure that you have the [required SDK components installed](https://taco.visualstudio.com/en-us/docs/configure-vs-tools-apache-cordova/#ThirdParty).
6. If there appears to be a problem with the Android SDK, you may need to re-install it. Before re-installing, delete the /User/username/.android and the /User/username/.gradle folder to make sure you get a fresh copy of the SDK. After [installing the SDK](http://go.microsoft.com/fwlink/?LinkID=396873), try again.

<a name="windows"></a>
##Troubleshooting Windows build and deployment issues

Check the Output, Error List, and JavaScript Console windows to see if you ran into one of the most common error messages on Windows.

* appxrecipe file missing?

    If you see this error when targeting Windows 10, make sure you set **Windows 10.0** as the target in the Windows tab of the configuration designer (config.xml). Then rebuild the project.
* Certificate error on Windows?

    Make sure your credentials are up to date. Check for any notifications or warning icons in the upper right of Visual Studio.

    ![Update your credentials](media/resolving-build-errors/windows-credentials.png)

    You may need to re-enter your credentials. If the notifications indicate that you need to update Cordova tooling, please click on the notifications and follow instructions.

* Content Security Policy is missing?

    Visual Studio will use the Cordova Whitelist plugin by default, so you need to update index.html in the Cordova app with the following `<meta>` element:

    ```
    <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: https://ssl.gstatic.com 'unsafe-eval'; style-src 'self' 'unsafe-inline'; media-src *">
    ```
    Depending on what resources your app needs to access, you may need to add a domain name or URI to the element.

* Unhandled exception running on Windows?

    If you see the following unhandled exception when targeting Win/WinPhone 10,  make sure you set **Windows 10.0** as the target in the Windows tab of the configuration designer.

    ![unhandled exception](media/resolving-build-errors/unhandled-exception.png)

    If you see same exception when targeting Win/WinPhone 8.1, follow the [steps here](https://taco.visualstudio.com/en-us/docs/tutorial-ionic/#unhandled) to call platformOverrides.js to fix this issue.

* WWAHost runtime error?

    When debugging on a Windows 8.1 dev machine, you may get a WWAHost runtime error when navigating between pages. This can happen when loading partial pages in some routing modules such as AngularJS. You can work around this by:

    * Closing DOM Explorer before navigating pages, or

    * Upgrading to Windows 10 on your dev machine (the platform issue is fixed in Windows 10).
