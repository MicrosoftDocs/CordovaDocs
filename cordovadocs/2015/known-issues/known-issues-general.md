--- 
description: "This article covers general known issues related to Visual Studio Tools for Apache Cordova."
title: "General Known Issues"
author: "jmatthiesen"
ms.prod: "visual-studio-dev14"
ms.author: jomatthi
--- 

# General Known Issues

> **Important**: We no longer maintain this article but if you’re stuck, ask us a question on [Stack using the tag ‘visual-studio-cordova'](http://stackoverflow.com/questions/tagged/visual-studio-cordova). Also, subscribe to our [developer blog](https://microsoft.github.io/vstacoblog/). We regularly post issues and workarounds.

This article covers general known issues related to Visual Studio Tools for Apache Cordova. For version-specific issues, see:

- [Known issues for Visual Studio 2015](./known-issues-vs2015.md)
- [Known issues for Tools for Apache Cordova CTP 3/3.1](./known-issues-vs2013.md)

## <strong>Build not executing when using Cordova with Node.js 5.0.0+ and Cordova 5.3.3 and below</strong>

If you use Cordova 5.3.3 or below with Node.js 5.0.0, you will see a build that appears to complete but does not actually execute due to an incompatibility between Cordova and this version of Node.

Cordova 5.3.3 and below does not support Node.js 5.0.0 or above. Either use Node.js 4.1.2 with Cordova 5.3.3 or Node.js 0.12.x with Cordova < 5.3.3. A planned update to Cordova (version > 5.3.3) will support Node.js 5.0.0+.

See [Safely update Node.js](../change-cordova-version/change-node-version.md) and [Change the CLI version of your Visual Studio Tools for Apache Cordova project](../change-cordova-version/change-cli-version.md).

## <strong>Build hangs or does not execute when building for iOS with Cordova < 5.3.3 and Node.js 4.0.0+</strong>

If you use Cordova < 5.3.3 or below with Node.js 4.x.x on your Mac, you will see iOS builds hang or simply succeed without actually executing.

Cordova 5.3.3 is the first version of Cordova that supports Node.js 4.x.x. Upgrade to Cordova 5.3.3 or higher or downgrade Node.js to 0.12.x if you need to use an earlier version of Cordova.  Note that 5.3.3 does not support Node.js 5.0.0+ so you will need to use a later version if you intend to use Node.js 5.0.0 or later.

See [Safely update Node.js](../change-cordova-version/change-node-version.md) and [Change the CLI version of your Visual Studio Tools for Apache Cordova project](../change-cordova-version/change-cli-version.md).

## <strong>Missing Platform Dropdown</strong>

The “Solution Platform” dropdown might not appear in the toolbar when upgrading Visual Studio 2013 from a previous version to Update 4. You can add using the “Standard Toolbar Options” drop-down as described in [Microsoft Support article 2954109](https://support.microsoft.com/kb/2954109).

## <strong>Building a Cordova project from source control results in Cordova plugin APIs not returning Results</strong>

The following four json files can cause this to occur if added to source control.

- plugins/android.json
- plugins/ios.json
- plugins/windows.json
- plugins/remote_ios.json
- plugins/wp8.json.

Remove these files from source control if you are not checking in the "platforms" folder (recommended). For local copies, you can either fetch a fresh copy from source control or remove the above files along with platforms found in the "platforms" folder to resolve the issue. See [tips and workarounds](../tips-workarounds/general-tips.md) for additional details.

## <strong>Plugin with variables not working</strong>

Due to a Cordova issue with Cordova 4.3.0 and 4.3.1, you can run into problems with plugin variables in Cordova versions prior to 5.0.0. Plugin variable information is lost if you install the "plugin" before the "platform" which can happen depending on your workflow. The variables do, however, work in Cordova 5.1.1, which you can use with VS 2015. Follow these steps to use a plugin with variables:

 1. Remove the plugins with the variables using the config designer.

 2. Update to Cordova 5.1.1 using the config designer (Platforms > Cordova CLI)

 3. Re-add your plugin via "Plugins" tab in the config.xml designer

## <strong>Slow first build or first plugin add</strong>

The first build or plugin that you add for a given version of Cordova will be slower than subsequent builds as VS must first dynamically acquire Cordova. See the Output Window for more detail on the progress. Furthermore, the first remote iOS build will exhibit the same behavior as the agent downloads Cordova on your OSX machine. If you encounter a CordovaModuleLoadError with the first iOS build for a given Cordova version, you can follow [these instructions](../tips-workarounds/ios-readme.md) to resolve the problem.

## <strong>Old versions of Cordova plugins due to Cordova plugin ID changes</strong>

A significant change occurred with Cordova 5.0.0+ that also altered the IDs of many core Cordova plugins. The Visual Studio 2015 config designer (config.xml) uses the old IDs (ex: org.apache.cordova.camera not cordova-plugin-camera) with Cordova 4.3.1 and below since the version of Cordova before 5.0.0 does not support npm.

If you update your Cordova version to 5.1.1 or later, the config designer will automatically switch to using the new IDs. If you do not see this behavior, update Tools for Apache Cordova. If you're an early adopter, you might not see some of the improvements described in this document until after you update since a small post-RTM update enabled this functionality. You will get an update notification soon that prompts you to update or, when creating a new project, you can click "Install Tools for Apache Cordova" from the Apache Cordova templates section. Be sure to remove plugins that use older IDs from your project before you add the updated plugins with the new IDs.

## <strong>Git sourced plugins will not install</strong>

Git sourced plugins will not install properly if you have not installed the [Git command line tools](http://www.git-scm.com/downloads) and have the tools in your system path. During installation of the Git tools, select the **Use Git from the Windows Command Prompt** option or add the "bin" folder from the Git install location to your path and restart VS. (Usually "C:\Program Files (x86)\Git\bin").

## <strong>Git sourced plugins will not install with Cordova 5.1.1 only</strong>

Cordova 5.1.1 has a bug that can cause plugins installed from a Git repo to fail with the error "Error: EXDEV, cross-device link not permitted" if the project is on a different drive than your temp folder.

See [tips and workarounds](../tips-workarounds/general-tips.md) for information about how to add plugins that are not present in the config designer. You can add them from either the Cordova plugin repository or npm. If you must add a Git version of the plugin, either move your project to the same drive as your temp folder when you install, or you can instead download a copy, unzip it, and add the plugin from the filesystem.

## <strong>TypeError: Request path contains unescaped characters</strong>

When building or installing a plugin, you might encounter this error if you are using a proxy with certain versions of Node.js and Cordova after a "npm http GET". This is a Cordova issue and the simplest workaround is to downgrade [Node.js 0.10.29](http://nodejs.org/dist/v0.10.29/). This will be resolved in a future version of Cordova. See [tips and workarounds](../tips-workarounds/general-tips.md) for additional details.

## <strong>Errors from npm related to permission problems</strong>

If you installed Visual Studio or Node.js as an administrator, you can run into problems where npm attempts to install npm packages under "C:\Program Files (x86)\node.js". You will typically see errors in the Output Window similar to this one: "npm ERR! error rolling back error : EPERM, unlink 'C:\Program Files (x86)\nodejs\vs-tac-cli.cmd'". To resolve this issue, you can re-install Node.js or reconfigure npm using these commands from the command prompt and restarting Visual Studio:

```
npm config set prefix %APPDATA%\npm
npm config set cache %APPDATA%\npm-cache
```

## <strong>Frequent ECONRESET errors from npm when using VPN or proxy</strong>

Node.js can experience intermittent issues when using SSL to connect to the npm repository and with certain versions of Node.js and npm when connected to VPN, or when a proxy is configured. You can resolve this issue by configuring npm to connect to the registry using straight HTTP instead using this command from the command prompt:

```
npm config set registry http://registry.npmjs.org
```

## <strong>FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed - process out of memory</strong>

This error can occur if you are using a recent version of Node.js (for example, 0.12.4) due to a [known npm issue](https://github.com/npm/npm/issues/8019). The simplest solution is to downgrade to [Node.js 0.10.29](http://nodejs.org/dist/v0.10.29/).

## <strong>Globally installed npm packages not available from command line</strong>

If you have installed Visual Studio or Node.js with a different user than you are logging into Windows, you may need to update your path to access globally installed (npm install -g) npm packages from the command line. Specifically, ensure **%APPDATA%\npm** is either in your user or system PATH. By default, the installation of Node.js will set the PATH as a user environment variable rather than system which is why you can encounter this behavior.

## <strong>TypeScript code incorrectly identified as external, cannot read user configuration file</strong>

When using TypeScript, there are known issues that will incorrectly identify project items as external code or will fail to read in a user configuration file. To avoid unexpected behavior when working with a Cordova TypeScript project, turn off Just My Code (**Options** > **Debugger** > **General** > uncheck **Enable Just My Code**).

## <strong>TypeScript breakpoints stop at the wrong location when using Ripple</strong>

This is a known issue that is being actively worked on. This problem does not occur when using devices or emulators.

## <strong>Missing Intellisense</strong>

- No IntelliSense is provided for Cordova plugins in JavaScript files in Apache Cordova projects. As a workaround, developers can enable IntelliSense for Cordova plugins by explicitly adding “/// &lt;reference group="Implicit (Multi-Device Apps)” /&gt;” to the JavaScript file.

- No IntelliSense is provided within JavaScript files for other JavaScript files that are included using a script tag in a referring HTML page. As a workaround, developers can enable IntelliSense for other referenced JavaScript files by explicitly adding “/// &lt;reference path=”referencedFile.js” /&gt;” to the JavaScript file.

## <strong>"res" folder contents cannot be referenced from web content</strong>

By design, the contents of the “res” folder cannot be accessed by web content as they are copied into platform-specific native project locations.

## <strong>Mixed case file added as lower case reference</strong>

Dragging a mixed-case file into an HTML page creates a lowercase script or style reference which will cause it to not be found on Android and iOS. Manually update the reference with the correct casing.

## <strong>Visual Studio hangs when creating or opening a Cordova jsproj</strong>

Follow the steps [in this MSDN blog post](https://social.msdn.microsoft.com/Forums/en-US/0e5115ca-83a7-4294-8740-289b3f453fca/rtm-known-issue-package-load-failure-when-creating-a-windows-app-project-with-javascript-or-hang).
