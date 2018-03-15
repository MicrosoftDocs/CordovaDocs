<properties pageTitle="Release Notes for Update 3"
  description="Release notes for Update 3 of Visual Studio 2015 Tools for Apache Cordova"
  services=""
  documentationCenter=""
  authors="Jordan" />
  <tags ms.technology="cordova" ms.prod="visual-studio-dev14"
     ms.service="na"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="mobile-multiple"
     ms.workload="na"
     ms.date="10/09/2015"
     ms.author="jomatthi"/>

#**Update 3 - Visual Studio Tools for Apache Cordova**
Update 3 of the Visual Studio Tools for Apache Cordova, includes the following changes:

* **Addresses a blocking issue when trying to use the Tools for Apache Cordova along with the ASP.NET 5 Beta 7 releas.**

  Previously, after installing ASP.NET 5 Beta 7, Cordova projects would fail when opening with an error message that says “An equivalent project (a project with the same global properties and tools version) is already present in the project collection…”  

* **Deploying to Windows Phone 8 devices now works when emulators are not installed.**

  Before this fix, Visual Studio showed an error saying that “no emulators are installed” and stopped deploying to the device. 
* **When working on an app for Android devices, an issue was fixed that caused a “could not reserve enough space for object heap” error.**

  Without this fix, an “Error occurred during initialization of VM; Could not reserve enough space for object heap” message appeared when using the Visual Studio Tools for Apache Cordova, targeting Android devices.

* **Fixed a time-out issues when deploying from Visual Studio to a remote iOS simulator.**

  Prior to this fix, Visual Studio would occasionally report a simulator time-out when there really wasn’t one.

* **The tsconfig.json file, used to configure TypeScript builds, may not be included at the root of a Cordova project.**

  Previously, this file had to be placed within a Scripts folder.

* **Fixes an issue where projects targeting Windows 10 break during a build after an app package has been created.**

  Without this fix, projects could be stuck with build errors after an app package was created, targeting Windows 10.

* **Disabled the option to add files to a Cordova project as links**

  Prior to this fix, files added to Cordova project as links (using the Add | New Item dialog) would show up initially, then disappear after build.
  
* **Found and fixed an internal error that caused Visual Studio to crash**

  Without this fix, there were cases that could cause a fatal exception which crashed Visual Studio.

* **Builds no longer fail when taco.json is missing**

  Previously, if taco.json was deleted in a project, the build would fail. Now if taco.json is missing, the build step adds the file back in.

* **Fix for Connect bug [1748409](https://connect.microsoft.com/VisualStudio/feedback/details/1748409/incorrect-cordova-android-versioncode) Incorrect Cordova Android versioncode**
* **Supported localization of an unlocalized Help link in config.xml**
* **When installing Visual Studio Tools for Apache Cordova, the latest version of Windows Tools is also selected**
* **On build, the vs:packageOutputPath setting in config.xml is now cleared so that absolute paths are not checked into source control**
 
