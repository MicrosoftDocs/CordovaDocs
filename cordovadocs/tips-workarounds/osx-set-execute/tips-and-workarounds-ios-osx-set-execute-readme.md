<properties pageTitle="Bower Tutorial"
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

#Workaround for Missing Execute Bit for Builds on OSX After Checking in Platforms Folder from Windows

License: MIT

If you receive errors that originate from files in your project's **platforms** folder when building either Android or iOS on OSX, the root cause might be that you checked in shell scripts under the "platforms/android/cordova", "platforms/ios/cordova", "platforms/windows/cordova", or "platforms/wp8/cordova" folders from Windows. This is because the NTFS file system has no concept of an "execute bit" that is required to run these from OSX. (The contents of the platforms is generally not intended for check-in and by default, they are excluded from Cordova projects in Visual Studio as a result.)

For example, this error is saying the "version" script is not executable:

```
[17:41:57] Error:
/Users/vsoagent/vsoagent/agent/work/build/b424d56537be4854de825289f019285698609afddf826d5d1a185eb60b806e47/repo/tfs-vnext test/platforms/android/cordova/version:
Command failed with exit code EACCES
```

To resolve this problem you have two options:

1.  Don't check in or copy the contents of the **platforms** folders. This is by far the path of least resistance.

2.  If you absolutely must check in the contents of the platforms folder from Windows, you can craft a simple script to set the execute bits on these files and include it as a part of your build process.
	1. Download [hook-execute-bit-fix.js](https://github.com/Microsoft/cordova-docs/tree/master/articles/tips-and-workarounds/ios/osx-set-execute) and drop it in a **hooks** folder in your project root.

	2. Update config.xml with the following (using right-click->**View Code**):

	  ```
	  <hook type="before_plugin_add" src="hooks/hook-execute-bit-fix.js" />
	  <hook type="after_platform_add" src="hooks/hook-execute-bit-fix.js" />
	  <hook type="before_prepare" src="hooks/hook-execute-bit-fix.js" />
	  ```

	2. Commit and check these into source control.

	3. Next time you build, run, and add a plugin, the problem should be resolved.

