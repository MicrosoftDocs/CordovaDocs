--- 
description: "This article covers known issues specific to Visual Studio Tools for Apache Cordova 2015."
title: "Known Issues - Visual Studio 2015"
author: "jmatthiesen"
ms.prod: "visual-studio-dev14"
ms.date: "09/10/2015"
ms.author: jomatthi
--- 

# <strong>Known Issues - Visual Studio 2015</strong>

> **Important**: We no longer maintain this article but if you’re stuck, ask us a question on [Stack using the tag ‘visual-studio-cordova'](http://stackoverflow.com/questions/tagged/visual-studio-cordova). Also, subscribe to our [developer blog](https://microsoft.github.io/vstacoblog/). We regularly post issues and workarounds.

This article covers known issues specific to Visual Studio Tools for Apache Cordova 2015.

## <strong>Project structure change from CTP3/3.1</strong>

Projects created in an earlier version of Visual Studio will need to be migrated to support the new Cordova CLI based project structure in VS 2015 that is more interoperable with 3rd party tools and CLIs.

To migrate your previous projects to the new structure:

* See [Migrate your project](../first-steps/migrate-to-vs2015.md).

## Visual Studio 2015 RTM
**The debug dropdown just shows 'Start' and no other target options appear such as Ripple, devices, emulators & simulators:**
In some cases, when you uninstall VS2013 or a previous version (RC) of VS2015, a library gets corrupted that causes the debug dropdown not to show all the target options. To resolve this issue:

1. Close all Visual Studio instances.
2. cd %appdata%\Local\Microsoft\Phone Tools”
3. Rename the CoreCon folder
4. Launch Visual Studio again.

--- --- --- -
**vs-ms-remote reports a 404 error when using VS 2015 RTM or later:** VS 2015 RTM and later versions use a new "remotebuild" agent instead of vs-mda-remote. See [remotebuild installation instructions](https://go.microsoft.com/fwlink/?LinkID=533745) for details.

--- --- --- -
**iOS Simulator does not work when using the remotebuild agent and VS 2015 RTM:** You need to install version 3.1.1 of the ios-sim node module. Run "npm install -g ios-sim@3.1.1" from the Terminal app in OSX to install it. See [remotebuild installation instructions](https://go.microsoft.com/fwlink/?LinkID=533745) for details.

--- --- --- -
**iPhone 4S Simulator appears when selecting iPad or other device when using the remotebuild agent and VS 2015 RTM:** You need to install version 3.1.1 of the ios-sim node module. Run "npm install -g ios-sim@3.1.1" from the Terminal app in OSX to install. See [remotebuild installation instructions ](https://go.microsoft.com/fwlink/?LinkID=533745) for details.

--- --- --- -
**Existing vs-mda-remote settings in Visual Studio do not work with the remotebuild agent:** You will need to generate and use a new PIN when setting up Visual Studio to connect to the remotebuild agent for the first time. If you are not using secure mode, turn secure mode on and then off again to cause VS to re-initalize. See [remotebuild installation instructions](https://go.microsoft.com/fwlink/?LinkID=533745) for details.

--- --- --- -
**Configuration Designer (config.xml) Does Not Show Updated Plugin IDs with Cordova 5.0.0+:** A significant change occurred with Cordova 5.0.0+ that also altered the IDs of many core Cordova plugins. The Visual Studio 2015 config designer uses the old IDs (ex: org.apache.cordova.camera not cordova-plugin-camera) with Cordova 4.3.1 and below since this version of the Cordova < 5.0.0 do not support npm.

However, if you update your Cordova version to 5.0.0 or later the config designer should automatically see the new IDs in the "Plugins" tab of the designer. If you do not see this behavior, update Tools for Apache Cordova because this functionality was added in a small post-RTM update. You will get an update notification soon prompting you to update or, when creating a new project, you can click "Install Tools for Apache Cordova" from the Apache Cordova templates section. To avoid unexpected behaviors, be sure to remove plugins with the older IDs before using the "Installed" tab to update the tools.

## Visual Studio 2015 RC
--- --- --- -
**VS 2015 RC and Cordova 5.x.x / Cordova Android 4.x.x:** See [Cordova 5.x.x known issues](known-issues-cordova5.md) for details on Android related issues that are specific to Cordova 5.0.0 and up.

--- --- --- -
**Old versions of Cordova plugins due to Cordova plugin ID changes:** A significant change occurred with Cordova 5.0.0+ that also altered the IDs of many core Cordova plugins. The Visual Studio 2015 RC config designer uses the old IDs (ex: org.apache.cordova.camera not cordova-plugin-camera) because Cordova 4.3.1 and previous versions cannot access plugins using these new IDs and the default template uses 4.3.0.

To install updated plugins, follow [this proceedure to install a npm sourced plugin](../tips-workarounds/general-tips.md).

*Note that these updated plugins were tested on Cordova 5.0.0 or later and therefore may or may not work on earlier versions of Cordova.* We advise against updating your plugins when using older versions of Cordova unless you are attempting to solve a specific problem.

--- --- --- -
**Building a Cordova project from source control results in Cordova plugin APIs not returning results:** The following four .json files can cause this to occur if added to source control.

- plugins/android.json
- plugins/ios.json
- plugins/windows.json
- plugins/remote_ios.json
- plugins/wp8.json.

Remove these files from source control if you are not checking in the "platforms" folder (reccomended). For local copies, you can either fetch a fresh copy from source control or remove the above files along with platforms found in the "platforms" folder to resolve the issue. See [tips and workarounds](../tips-workarounds/general-tips.md) for additional details.

--- --- --- -
**Plugin with variables not working:** Due to a Cordova issue with Cordova 4.3.0 and a bug in Visual Studio 2015 RC, you can run into problems with plugin variables in Cordova versions prior to 5.0.0. Plugin variable information is lost if you install the "plugin" before the "platform", which can happen depending on your workflow. However, plugin variables do function in Cordova 5.1.1, which you can use with VS 2015 RC. To update to 5.1.1 and use plugin variables, you will need to update your VS project and use the command line.

 1. Remove the plugins with the variables via the config designer.

 2. Update to Cordova 5.1.1 via the config designer (Platforms > Cordova CLI)

 3. From the command line:
	 1. Go to your project directory.
	 2. Type the following from substituting project path, plugin name, and variables for those that apply to you:

	    ~~~~~~~~~~~~~~
        cd <project path>
		npm install -g cordova@5.1.1
        cordova plugin add nl.x-services.plugins.launchmyapp --variable URL_SCHEME=myscheme
	    ~~~~~~~~~~~~~~

## Visual Studio 2015 CTP6
--- --- --- -
**Ant uninstalled when upgrading from CTP5 to CTP6:** When you upgrade from VS2015 CTP5 to CTP6, Apache Ant gets uninstalled. The workaround is to reinstall Ant. You can find manual instructions for installing and configuring Ant at [this location](https://msdn.microsoft.com/library/dn757054.aspx#InstallTools).
