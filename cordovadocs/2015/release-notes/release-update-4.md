<properties pageTitle="Release Notes for Update 4"
  description="Release notes for Update 4 of Visual Studio 2015 Tools for Apache Cordova"
  services=""
  documentationCenter=""
  authors="Linda" />
  <tags ms.technology="cordova" ms.prod="visual-studio-dev14"
     ms.service="na"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="mobile-multiple"
     ms.workload="na"
     ms.date="10/26/2015"
     ms.author="lizhong"/>

#**Update 4 - Visual Studio Tools for Apache Cordova**

##Feature updates:

**Chain Android SDK v23 into VS TACO feed**

The newest version of Android SDK is automatically included with a VS TACo install.

---------------------------

**Plugin Installation Log**

When a developer installs a plugin using the config designer in Visual Studio, a log window is opened with the log messages from Cordova. This should help developers with plugin installation debugging. 

---------------------------

**TypeScript Debugging in Chrome Dev Tools**

We added the inlineSources property to the project templates’ tsconfig.json file to help the Chrome developer tools debug TypeScript projects.

---------------------------

##Bug fixes:

**Launching the Chrome Dev tools when VS is attached no longer closes Chrome**

Bug: After launching the Ripple emulator in debug mode, opening Chrome dev tools (hitting F12) closes Chrome. This is due to the fact that Chrome sends a “disconnect” event both when it exits and when dev tools launch. 
Fix: Instead of listening for the “disconnect” event, VS listens for the Chrome process to end and use that as the trigger to end ripple.

---------------------------

**Can now “Drag and drop” Toolbox items on to HTML**

Bug: After opening an HTML file, when you try to drag a tool from the Toolbox onto the file, the "no smoking" cursor remains and the drop doesn't work.
Fix: The tool is applied to the HTML file. 

---------------------------

**Success during secure mode for iOS build, and accompanying certificate generates**

Bug: If the user has options stored for using secure mode for iOS build, but did so without generating the behind-the-scenes properties, then the build will fail immediately and with no error message. This is due to the failure to generate the CerficateName property during the build.
Fix: When the user builds using secure mode for iOS build, all properties will set correctly, leading to a successful build. 

---------------------------

**Can debug Google Android Emulator when VS emulator is running, or device is attached**

Bug: After detaching from VS Android emulator to start debugging on the Google Android emulator, debugger attaches to a new adb.exe process instead.
Fix: Debugger now attaches correctly to the Google Android emulator and runs as expected.

---------------------------

**DOM Explorer attaches properly even if InAppBrowser appears at app startup**

Bug: Running the VS Android emulator when the VS Android emulator is already running causes VS to connect to second webview containing _iab.html instead of the main webview.
Fix: Fixed the launch URI

---------------------------

**Config file correctly parses plugins when no version is specified**

Bug: After manually editing config.xml to add a plugin with no "version" attribute, changeList.json does not have the correct added/removed plugins.
Fix: Added edge case in config.xml parsing.

---------------------------

**Can now deploy to Android an app that opens on a non-start page**

Bug: An app that launches a new window on OnDeviceReady() does not attach to the JS debugger.
Fix: Debugger can now handle this.

---------------------------

**Can attach Android device while VS is running**

Bug: Attaching an Android device while VS is running generates an error message about adb not "ACK"ing comes up, and no deployment occurs
Fix: Deployment now occurs.

