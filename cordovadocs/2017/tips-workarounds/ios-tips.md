--- 
title: "iOS and OSX Tips and Workarounds"
description: " Tips and workarounds for building iOS apps using Visual Studio Tools for Apache Cordova."
ms.prod: "visual-studio-dev15"
ms.devlang: "javascript"
ms.tgt_pltfrm: "mobile-multiple"
ms.workload: "na"
ms.topic: "troubleshooting"
ms.date: "09/10/2015"
--- 

# iOS and OSX Tips and Workarounds

<a name="symlink"></a>
## Errors About Missing Header or Library Files in Plugins

There are a small number of Cordova plugins that contain "custom framework" files for iOS. These use symlinks on OSX. Symlinks can break when the plugin is either downloaded on Windows and then moved to an OSX machine or when the plugin is pulled from the Cordova plugin repo / npm without the symlinks being present in the archive [as described in this Cordova bug](https://issues.apache.org/jira/browse/CB-6092). However, there is a simple hook that can be added to your project to work around this issue.

To install it:

1. Download [hook-symlink-fix.js](https://github.com/Microsoft/cordova-docs/tree/master/articles/tips-and-workarounds/ios/ios-plugin-symlink-fix) and drop it a **hooks** folder in your project root.
2. Update config.xml with the following (using right-click->**View Code**):

   ```
   <hook type="before_compile" src="hooks/hook-symlink-fix.js" />
   ```

3. Run a "Clean" operation in Visual Studio or remove the iOS platform, and then re-add it if you are using the command line.

## Permission Issues (EACCES Errors)
<a name="npm-cache"></a>
### Permission errors from "npm" when trying to build on the remote agent, a CI server, or the command line
If you are seeing permission errors from "npm," you may be running into a situation where the your user's cache folder (~/.npm) is inaccessible. Generally this occurs if the folder or some of its contents was created while running as an administrator (sudo) when using older versions of Node.js and npm (though recent versions do not exhibit this behavior). Fortunately this is easy to resolve. Open the Terminal app and type:

```
sudo chown -R `whoami` ~/.npm
```

<a name="osx-set-execute"></a>

### Permission errors when building or executing a cordova command (plugin add) on OSX after checking in or copying the "platforms" folder from Windows

If you are seeing errors that are originating from files in your project's "platforms" folder when building either Android or iOS on OSX, the root cause may be that you checked in shell scripts under the "platforms/android/cordova", "platforms/ios/cordova", "platforms/windows/cordova", or "platforms/wp8/cordova" folders from Windows.

This is because the NTFS file system has no concept of an "execute bit" that is required to run these from OSX. (The contents of the platforms folder is generally not intended for check-in, and by default, they are excluded from Cordova projects in Visual Studio as a result.)

For example, this error is saying the "version" script is not executable:

```
[17:41:57] Error:
/Users/vsoagent/vsoagent/agent/work/build/b424d56537be4854de825289f019285698609afddf826d5d1a185eb60b806e47/repo/tfs-vnext test/platforms/android/cordova/version:
Command failed with exit code EACCES
```

To resolve this problem you have two options:

1. Don't check in or copy the contents of the **platforms** folder. This is by far the path of least resistance.

2. If you absolutely must check in the contents of the **platforms** folder from Windows, you can craft a simple script to set the execute bits on these files and include it as a part of your build process.
   1. Download [hook-execute-bit-fix.js](https://github.com/Microsoft/cordova-docs/tree/master/articles/tips-and-workarounds/ios/osx-set-execute) and drop it in a **hooks** folder in your project root.

   2. Update config.xml with the following (using right-click->**View Code**):

	  ```
	  <hook type="before_plugin_add" src="hooks/hook-execute-bit-fix.js" />
	  <hook type="after_platform_add" src="hooks/hook-execute-bit-fix.js" />
	  <hook type="before_prepare" src="hooks/hook-execute-bit-fix.js" />
	  ```

   3. Commit and then check these into source control.

   4. Next time that you build, run, and then add a plugin, the problem should be resolved.
