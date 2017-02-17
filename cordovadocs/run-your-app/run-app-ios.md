<properties
   pageTitle="Run your Apache Cordova app on iOS | Cordova"
   description="description"
   services="na"
   documentationCenter=""
   authors="Mikejo5000"
   tags=""/>
<tags ms.technology="cordova" ms.prod="visual-studio-dev14"
   ms.service="na"
   ms.devlang="javascript"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="na"
   ms.date="05/05/2016"
   ms.author="mikejo"/>
# Run your Apache Cordova app on iOS

When you create app using Visual Studio Tools for Apache Cordova, you have many options for emulating and deploying your app on iOS:

*   [Apache Ripple simulator](run-app-ripple-simulator.md), which provides basic validation of layout and CSS. (This does not require installation of the remote agent for iOS.)

*   [iOS Simulator](#iOSSimulator), which requires installation and configuration of the [remote agent](../getting-started/ios-guide.md) when running from Visual Studio.

*   iOS Simulator using a service such as [MacInCloud](../getting-started/build_ios_cloud.md), which allows you to install the remote agent without a physical Mac device.

*   [iOS device](#iOSDevice)</span>, which also requires installation and configuration of the [remote agent](../getting-started/ios-guide.md) when running from Visual Studio.

If you run into errors building for iOS, see these [tips and workarounds](../tips-and-workarounds/tips-and-workarounds-readme.md) and these [known issues](../known-issues/known-issues-ios.md).

If you have configured your Mac as described in the article [iOS Setup Guide](../getting-started/ios-guide.md), you can build and run your app on the iOS Simulator. The iOS Simulator runs on your Mac.

>**Note**: We recommend that you use the iOS Simulator if you have a personal Mac or if you’re running Windows from a Mac using virtualization technology such as Parallels .

### To run your app on the iOS Simulator <a name="iOSSimulator"></a>

1.   Make sure that you have [installed required software](../getting-started/ios-guide.md#install), including the remote agent, on a Mac that meets all system requirements, and that you have started the remote agent.

2.   Make sure that you have [specified iOS processing](../getting-started/ios-guide.md#getInfo), the host name or IP address, port, and security PIN in Visual Studio.

3.  With your app open in Visual Studio, choose **iOS** from the **Solution Platforms** list. If you don’t see this list, choose **Solution Platforms** from the **Add/Remove Buttons** list to display it.

4.  In the simulator list, choose **Simulator – iPhone 5** or another iOS simulator.

    ![Selecting the iOS Simulator](media/run-app-ios/run-ios-simulator-select.png)

5.  Press F5 to start the app.

    If the remote agent is set up correctly, Visual Studio deploys the app to the iOS Simulator on your Mac.

    ![iOS Simulator running on a Mac](media/run-app-ios/run-ios-simulator.png)

**Tip** If you are running a version of the iOS Simulator with a Retina display on a lower-resolution Mac with a non-Retina display, you may need to scale the emulator to 50% so the app will fit on the screen (choose **Window**, **Scale**).

If you have configured the **remote build** agent on your Mac as described in the article [iOS Setup Guide](../getting-started/ios-guide.md), you can build and run your app on an iOS device that is connected to your Mac or PC.

### Troubleshooting? Let's fix it

See the troubleshooting tips in the [iOS Guide](../getting-started/ios-guide.md)

### To run your app on an iOS device <a name="iOSDevice"></a>

1.  If you are running your app on a device that is connected to your Windows PC, make sure that you have installed iTunes on the PC.

	iTunes is not required if you are running your app on a device that is connected to your Mac.

2. Make sure that you have [installed required software](../getting-started/ios-guide.md#install), including the remote agent, on a Mac that meets all system requirements, and that you have started the remote agent.

3.  Make sure that the iOS device has been provisioned with the same provisioning profile as the Mac. For more information about provisioning, see [Create a provisioning profile](../getting-started/ios-guide.md#create-a-provisioning-profile).

4.  Make sure that you have [specified iOS processing](../getting-started/ios-guide.md#getInfo), the host name or IP address, port, secure mode, and security PIN in Visual Studio.

5.  With your app open in Visual Studio, choose **iOS** from the **Solution Platforms** list.

    If you don’t see this list, choose **Solution Platforms** from the **Add/Remove Buttons** list to display it.

6.  In the device list, choose **Local Device** to run your app on a device connected to your PC, or choose **Remote Device** to run your app on a device connected to your Mac.

    ![Selecting an iOS device](media/run-app-ios/run-ios-device-select.png)

7.  Press F5 to start the app.

    If the remote agent and is set up correctly and your iOS device is connected, the app will run on your device.

    If you are running the app on your PC (**Local Device**), iTunes will start and the iOS app will automatically be added to (or replaced in) your library. You can [install or update the app](http://support.apple.com/kb/PH12315) on a connected device as you would any other iOS app.

![Download the tools](media/run-app-ios/run-ios-download-link.png) [Get the Visual Studio Tools for Apache Cordova](http://aka.ms/mchm38) or [learn more](https://www.visualstudio.com/cordova-vs.aspx)
