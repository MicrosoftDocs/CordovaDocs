<properties pageTitle="Windows tips and workarounds"
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
     ms.date="05/15/2016"
     ms.author="kirupac"/>

#Windows tips and workarounds
This document covers tips, tricks, and known workarounds for problems with the Cordova Windows and Windows Phone platform. 

<a name="windows"></a>
##Resolve Windows build and deployment errors

Try these steps if you have trouble building and deploying to Windows or Windows Phone emulators or devices.

1. Before taking any other steps, try running your app against other platforms or emulators to make sure the issue is Windows-specific.

    For example, we suggest you test on Android:
    * Try running against Android and select **Ripple - Nexus (Galaxy)** (Chrome required)

    If you see the same error on other platforms, the issue is likely not Windows-specific, see [Resolve build and deployment errors](../general/tips-and-workarounds-general-readme.md) for more general help.

2. Open the configuration designer (config.xml), Windows tab, and make sure you select the OS that your are currently trying to target (Windows 8.1 or Windows 10).

3. Running on a device? Make sure that your device is enabled for development. Instructions to do this are different for each platform.
        * For Windows Phone, see [this article](../../develop-apps/run-app-windows-phone.md). For Windows 10, Visual Studio will prompt you to enable the device.

4. If you are running on a device and the app is already installed, uninstall the app and try again.

5. Check the Output, Error List, and JavaScript Console windows to see if you ran into one of the most common error messages on Windows (see the following questions and issues).

Here are some of the more common issues that you might see on Windows.

* appxrecipe file missing?

    If you see this error when targeting Windows 10, make sure you set **Windows 10.0** as the target in the Windows tab of the configuration designer (config.xml). Then rebuild the project.
* Certificate error on Windows?

    Make sure your credentials are up to date. Check for any notifications or warning icons in the upper right of Visual Studio.

    ![Update your credentials](media/tips-and-workarounds-windows-readme/windows-credentials.png)

    You may need to re-enter your credentials. If the notifications indicate that you need to update Cordova tooling, please click on the notifications and follow instructions.

* Content Security Policy is missing?

    Visual Studio will use the Cordova Whitelist plugin by default, so you need to update index.html in the Cordova app with the following `<meta>` element:

    ```
    <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: https://ssl.gstatic.com 'unsafe-eval'; style-src 'self' 'unsafe-inline'; media-src *">
    ```
    Depending on what resources your app needs to access, you may need to [add a domain name or URI](https://www.npmjs.com/package/cordova-plugin-whitelist) to the `<meta>` element.

* Unhandled exception running on Windows?

    If you see the following unhandled exception when targeting Win/WinPhone 10,  make sure you set **Windows 10.0** as the target in the Windows tab of the configuration designer.

    ![unhandled exception](media/tips-and-workarounds-windows-readme/unhandled-exception.png)

    If you see same exception when targeting Win/WinPhone 8.1, follow the [steps here](https://taco.visualstudio.com/en-us/docs/tutorial-ionic/#unhandled) to call platformOverrides.js to fix this issue.

* WWAHost runtime error?

    When debugging on a Windows 8.1 dev machine, you may get a WWAHost runtime error when navigating between pages. This can happen when loading partial pages in some routing modules such as AngularJS. You can work around this by:

    * Closing DOM Explorer before navigating pages, or

    * Upgrading to Windows 10 on your dev machine (the platform issue is fixed in Windows 10).

## More Information
* [Read tutorials and learn about tips, tricks, and known issues](../../cordova-docs-readme.md)
