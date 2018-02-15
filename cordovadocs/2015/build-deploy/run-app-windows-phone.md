---
title: "Run your Apache Cordova app on Windows Phone | Cordova"
author: "jmatthiesen"
ms.prod: "visual-studio-dev14"
ms.date: "09/10/2015"
ms.author: "jmatthiesen"
---

# Run your Apache Cordova app on Windows Phone


Visual Studio provides these two options for deploying your app built with Visual Studio Tools for Apache Cordova on Windows Phone:

*   Windows Phone emulator

*   Windows Phone device

Windows 8, Windows 8.1, Windows 10, or Windows Server 2012 R2 (with Desktop Experience enabled) is required to deploy and run your app on Windows Phone.

You can choose either Windows Phone 8 or Windows Phone (Universal) as your deployment target. When you choose Windows Phone (Universal), the generated project is an APPX package, which is a native Windows Store app targeting Windows Phone 8.1 or Windows 10. If you choose Windows Phone 8, the generated project is a XAP package; Windows Phone 8 apps for Cordova are hosted in a Silverlight WebView control.

>**Note**: You can attach the Visual Studio debugger when targeting Windows Phone 8.1 or Windows 10, but not Windows Phone 8.

Follow these instructions to run your app on the Windows Phone emulator. For additional information, see [Run Windows Phone apps in the emulator](https://msdn.microsoft.com/library/windows/apps/dn632391.aspx) in the Windows Dev Center.

### To run your app on the emulator

1. Make sure that Hyper-V is enabled on your PC. Your PC must support the Windows Phone emulator [system requirements](https://msdn.microsoft.com/library/windowsphone/develop/ff626524.aspx).

2. With your app open in Visual Studio, open config.xml, choose the **Windows** tab, and select either **Windows 8.1** or **Windows 10** in the **Target Platform** drop-down box.

    >**Note**: This config.xml option is not used to target Windows Phone 8.

3.  With your app open in Visual Studio, choose **Windows Phone 8** or **Windows Phone (Universal)** from the **Solution Platforms** list.

    ![Selecting your target platform](./media/run-app-windows-phone/run-windows-phone-sol-platforms.png)

    If you don’t see this list, choose **Solution Platforms** from the **Add/Remove Buttons** list to display it.

4.  Choose one of the emulators, such as **Mobile Emulator 10.x.xxxxx.x WVGA 512MB**.

5.  Press F5 to start the app.

    Visual Studio starts the emulator and runs the app.

    ![Running an app on the Windows Phone Emulator](./media/run-app-windows-phone/run-windows-phone-simulator.png)

Follow these instructions to run your app on a Windows Phone device that is connected to your PC.

### Troubleshooting? Let's fix it

If you have trouble deploying to Windows Phone, see [Resolve Windows build and deployment errors](../tips-and-workarounds/windows-tips.md).

### To run your app on a device

1. Connect your device to your PC hosting Visual Studio.

2. Unlock your device for development.

    For Windows Phone 10, see [enable your device for development](https://msdn.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development). For Windows Phone 8, see [get a developer license](https://msdn.microsoft.com/library/windows/apps/hh974578) and [register your phone](https://msdn.microsoft.com/en-us/library/windows/apps/dn614128).

3. With your app open in Visual Studio, open config.xml, choose the **Windows** tab, and select either **Windows 8.1** or **Windows 10** in the **Target Platform** drop-down box.

4. With your app open in Visual Studio, choose **Windows Phone 8** or
**Windows Phone (Universal)** from the **Solution Platforms** list. 	If you don’t see this option, choose **Solution Platforms** from the **Add/Remove Buttons** list to display it.

5.  Choose **Device**.

6.  Press F5 to start the app.
	Visual Studio starts the app on the connected Windows Phone device.

### Troubleshooting? Let's fix it

If you have trouble deploying to Windows Phone, see [Resolve Windows build and deployment errors](../tips-and-workarounds/windows-tips.md).

![Download the tools](./media/run-app-windows-phone/run-windows-phone-download-link.png) [Get the Visual Studio Tools for Apache Cordova](http://aka.ms/mchm38) or [learn more](https://www.visualstudio.com/cordova-vs.aspx)

### See Also

[Debug Your App Built with Visual Studio Tools for Apache Cordova](../debug-test/visual-studio-unit-testing-with-chutzpah.md)
