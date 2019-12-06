---
title: "Known Issues - iOS"
author: "jmatthiesen"
ms.prod: "visual-studio-dev14"
ms.date: "09/10/2015"
ms.author: jomatthi
---

# <strong>Known Issues - iOS</strong>

> **Important**: We no longer maintain this article but if you’re stuck, ask us a question on [Stack using the tag ‘visual-studio-cordova'](http://stackoverflow.com/questions/tagged/visual-studio-cordova). Also, subscribe to our [developer blog](http://microsoft.github.io/vstacoblog/). We regularly post issues and workarounds.

This article covers known issues related to Visual Studio Tools for Apache Cordova 2015 when building or deploying to iOS devices or simulators.

----------
## <strong>ITMS-90474, ITMS-90475, and/or ITMS-90339 errors when submitting to the Apple App Store</strong>
When attempting to submit a Cordova app to the App Store created using Xcode 7 with Cordova 5.3.3 and below, you may encounter three errors: ITMS-90474, ITMS-90475, and ITMS-90339. These errors have to do with changes in store acceptance criteria by Apple.  There are two steps to resolve them.

For ITMS-90474 and ITMS-90475, a Cordova community member has published a "cordova-plugin-ipad-multitasking" plugin with a fix.

http://npmjs.com/package/cordova-plugin-ipad-multitasking

Install this plugin to resolve ITMS-90474, ITMS-90475 in Cordova 5.3.3 and below. Future versions of Cordova will solve this problem.

To resolve ITMS-90339 with Cordova 5.3.3 and below you will need to do the following:

1.	Grab the build.xcconfig from [the 3.9.x branch of the cordova-ios repo](https://raw.githubusercontent.com/apache/cordova-ios/3.9.x/bin/templates/scripts/cordova/build.xcconfig) and place this under res/native/ios/cordova
2.	Now remove this line:

~~~~~~~~~~~~~~~~~~~~~~~~
CODE_SIGN_RESOURCE_RULES_PATH = $(SDKROOT)/ResourceRules.plist
~~~~~~~~~~~~~~~~~~~~~~~~

Note that you will want to remove this custom build.xcconfig file if you upgrade to the version with the full patch that is forthcoming.  This change will cause problems on future versions of Cordova (specifically cordova-ios 4.0.0 and up).

## <strong>"You must rebuild it with bitcode enabled (Xcode setting ENABLE_BITCODE)" error in Output Window when building with Xcode 7 and certain Cordova plugins</strong>
This is a [minor incompatibility](https://issues.apache.org/jira/browse/CB-9721) with Cordova 5.3.3 below and Xcode 7.  Workaround:

1. Grab the build.xcconfig from [the 3.9.x branch of the cordova-ios repo](https://raw.githubusercontent.com/apache/cordova-ios/3.9.x/bin/templates/scripts/cordova/build.xcconfig) and place this under res/native/ios/cordova
2. Add…

   ~~~~~~~~~~~~~~~~~~~~~~~~
   ENABLE_BITCODE=NO
   ~~~~~~~~~~~~~~~~~~~~~~~~

Note that you will want to remove this custom build.xcconfig file if you upgrade to the version with the full patch that is forthcoming.

## <strong>"Include of non-modular header inside framework module" error in Output Window when building with Xcode 7 and certain Cordova plugins</strong>
This is a [minor incompatibility](https://issues.apache.org/jira/browse/CB-9719) with Cordova 5.3.3 below and Xcode 7.  Workaround:

1. Grab the build.xcconfig from [the 3.9.x branch of the cordova-ios repo](https://raw.githubusercontent.com/apache/cordova-ios/3.9.x/bin/templates/scripts/cordova/build.xcconfig) and place this under res/native/ios/cordova
2. Add…

   ~~~~~~~~~~~~~~~~~~~~~~~~
   CLANG_ALLOW_NON_MODULAR_INCLUDES_IN_FRAMEWORK_MODULES = YES
   ~~~~~~~~~~~~~~~~~~~~~~~~

Note that you will want to remove this custom build.xcconfig file if you upgrade to the version with the full patch that is forthcoming.

## <strong>Building for iOS hangs when Node.js v4.0+ is installed with Cordova < 5.3.3</strong>
There is a [known compatibility issue](https://issues.apache.org/jira/browse/CB-9297) with Node.js v4 in Cordova v5.3.1 and earlier. If you have installed Node.js v4 on your build Mac, and then installed Cordova and the remotebuild agent, iOS builds can fail in the following situations:
* The <name> element in config.xml has been changed (for example, you’re changing the name of your app).
* A <deployment-target> element is defined in config.xml (set when using the Deployment Target dropdown in the Config file UI in Visual Studio).
* A <target-device> element is defined in config.xml (set when using the Target iOS Version field in the Config file UI in Visual Studio).

We recommend that you do not use Node.js v4 on Macs running remotebuild unless you upgrade your projects to Cordova 5.3.3 or later.

## <strong>Incremental builds with remotebuild@1.0.1 and Visual Studio 2015 RTM is broken</strong>
*Note: This was resolved in VS 2015 Tools for Apache Cordova Update 1 - We reccomend updating to the latest update rather than following this workaround.*

VS 2015 RTM (no Tools for Apache Cordova updates) and remotebuild agent version 1.0.1 has a bug where incremental changes made to any files under the /www folder do not get updated/built on iOS.

*Observation:*

1.	First F5 = success
2.	Make changes to any files inside /www
3.	Second F5 = changes to /www don’t appear; looks exactly like the first F5


***Temporary Workaround A:***

1. Open File Explorer and Navigate to %APPDATA%\npm\node_modules\vs-tac\lib\
2. Replace file remoteBuild.js with one from [here](https://raw.githubusercontent.com/Microsoft/cordova-docs/master/known-issues/ios-remote-incremental-build-fix/remoteBuild.js)

> **Note**: if you are not using default npm installation location, then to find out the directory where remoteBuild.js is located, run “npm config get prefix” (from a command prompt) to get the base of the directory, “C:\Users\<user name>\AppData\Roaming\npm” for me, and then replace “\node_modules\vs-tac\lib\remoteBuild.js”.*

***Temporary Workaround B:***

1. First F5 = success
2. User makes changes to /www
3. **Do a Clean build or Rebuild**
4. Second F5 = success

## <strong>404 error when using vs-mda-remote with VS 2015 RTM or later</strong>

VS 2015 RTM and later versions a new "remotebuild" agent instead of vs-mda-remote. See [remotebuild installation instructions](http://go.microsoft.com/fwlink/?LinkID=533745) for details.

## <strong>iOS Simulator does not work when using the remotebuild agent and VS 2015 RTM</strong>
*Note: This has been resolved in the latest version of remotebuild. Install the latest version to avoid this problem.*

You need to install version 3.1.1 of the ios-sim node module. Run "npm install -g ios-sim@3.1.1" from the Terminal app in OSX to install. See [remotebuild installation instructions](../first-steps/ios-guide.md) for details.

## <strong>iPhone 4S Simulator appears when selecting iPad or other device when using the remotebuild agent and VS 2015 RTM</strong>
*Note: This has been resolved in the latest version of remotebuild. Install the latest version to avoid this problem.*

You need to install version 3.1.1 of the ios-sim node module. Run "npm install -g ios-sim@3.1.1" from the Terminal app in OSX to install. See [remotebuild installation instructions ](../first-steps/ios-guide.md) for details.

## <strong>Existing vs-mda-remote settings in Visual Studio do not work with the remotebuild agent</strong>

You will need to generate and use a new PIN when setting up Visual Studio to connect to the remotebuild agent for the first time. If you are not using secure mode, turn secure mode on and then off again to cause VS to reinitalize. See [remotebuild installation instructions](../first-steps/ios-guide.md) for details.

## <strong>CordovaModuleLoadError from iOS remote agent</strong>

This error can occur if your ~/.npm folder or some of its contents were created while running as an administrator (sudo). To resolve, run the following command after installing the latest version of the [remotebuild](https://www.npmjs.com/package/remotebuild) or [vs-mda-remote](https://www.npmjs.com/package/vs-mda-remote) packages. This command ensures that your user has permissions to the contents of the npm package cache in your home directory when using older versions of Node.js and npm. Newer versions of Node.js and npm will do this for you automatically.

    sudo chown -R `whoami` ~/.npm

## <strong>Slow first build</strong>
The first build using the remote iOS build agent for a given version of Cordova will be slower than subsequent builds as the remote agent must first dynamically acquire Cordova on OSX.

## <strong>Adding "plugins/&lt;platform&gt;.json" or "plugins/remote_ios.json" to source control can result in non-functional plugins</strong>

Five .json files that can cause issues if added to source control are missing from the default source code exclusion list including "plugins/remote_ios.json." If you encounter a build that has non-functional Cordova APIs after fetching the project from source control, you should ensure that "plugins/android.json", "plugins/ios.json", "plugins/windows.json", "plugins/remote_ios.json", and "plugins/wp8.json" are removed from source control and retry. See this [Tips and Workarounds](../tips-workarounds/general-tips.md) for additional details.

## <strong>Deploying to iOS 8.3 device fails from OSX Mavericks or below</strong>

If deploying to iOS 8.3 device fails because vs-mda-remote cannot find DeveloperDiskImage.dmg, make sure you are running OSX Yosemite and Xcode 6.3. Xcode 6.3 is required to deploy to an 8.3 device and only runs on Yosemite.

## <strong>"Could not find module 'Q'" error when building iOS</strong>

If your OSX machine has a case sensitive file system, you can hit this error with certain versions of Cordova like Cordova 5.1.1. (Most people do not turn on case sensitivity.) A fix is in the works and will be in the next version of the Cordova iOS platform along with an updated version of Cordova itself. Watch the [Cordova homepage](http://cordova.apache.org) for release announcements. Once the Cordova iOS platform is released, you can follow [these directions](../tips-workarounds/general-tips.md) to use it at release or you may wait until a full Cordova "tools" release also occurs and update the Cordova version using the config designer.

## <strong>Incremental builds not faster than initial build when using VS 2015 RC or RTM</strong>

Unfortunately, this is a known issue with the iOS incremental build feature. We are actively working on a fix that will be resolved in a point release update.

## <strong>Unresponsive iOS device during app deployment</strong>

In some circumstances, when deploying to iOS devices, the phone may enter an unresponsive state where apps may stop responding. Avoid deploying an app when the same app is still running.
As a workaround, if you enter this state, soft reset your iOS device.

## <strong>Errors about missing header or library files in plugins</strong>

There are a small number of Cordova plugins that contain "custom framework" files for iOS which use symlinks on OSX. Symlinks can break when the plugin is downloaded on Windows and then moved to an OSX machine. See this [Tips and Workarounds](../tips-workarounds/ios-readme.md) article for a fix.

## <strong>Custom iOS Simulator targets not in dropdown</strong>

Not all iOS Simulator devices are currently listed in the Debug Target dropdown in Visual Studio. A workaround is to manually change the device using the iOS Simulator Hardware > Device menu.

## <strong>iOS Simulator just shows white screen</strong>

This is likely because the iOS device being used has a resolution higher than the screen you are currently using and thus you only see the center of the web page. Use the Window > Scale menu to scale the content to fit.

## <strong>Plugin native code still present after removing plugin after incremental iOS build</strong>

If a plugin is added to your project, built for iOS, and then removed from the project, the plugin will still be included in the iOS build until you clean or build for another platform. As a workaround, clean or rebuild from Visual Studio instead of using build/debug.

## <strong>Failed to launch application on iOS remote build machine</strong>
Sometimes, you would see following output in the Visual Studio error pane:

    Http 404: Unable to launch app

 In some cases, this *may* occur if the app id in the widget element of config.xml (e.g. io.cordova.helloworld) has any non-ascii characters in it, or contains the substring '.app'. This is a known imitation of the iOS platform. To resolve this issue, you should modify the application id not to contain .app in the value or have ascii characters as the app id.
