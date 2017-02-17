<properties pageTitle="Cordova plugin Symlink fix for iOS plugins with custom frameworks"
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

#Cordova plugin Symlink fix for iOS plugins with custom frameworks

License: MIT

There are a small number of Cordova plugins that contain "custom framework" files for iOS. These use symlinks on OSX. Symlinks can break when the plugin is either downloaded on Windows and then moved to an OSX machine or when the plugin is pulled from the Cordova plugin repo / npm without the symlinks being present in the archive [as described in this Cordova bug](https://issues.apache.org/jira/browse/CB-6092). 

A telltale sign of this problem is the Cordova build for iOS reports missing "Header" files that are located inside one or more plugin paths.

However, there is a simple hook that can be added to your project to work around this issue.

To install it:

1. Download [hook-symlink-fix.js](https://github.com/Microsoft/cordova-docs/tree/master/articles/tips-and-workarounds/ios/ios-plugin-symlink-fix) and drop it a **hooks** folder in your project root.
2. Update config.xml with the following (using right-click->**View Code**):

  ```
  <hook type="before_compile" src="hooks/hook-symlink-fix.js" />
  ```

3. Run a "Clean" operation in Visual Studio or remove the iOS platform, and then re-add it if you are using the command line.

