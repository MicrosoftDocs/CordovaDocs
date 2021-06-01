--- 
title: "Manage plugins for apps built with Apache Cordova"
description: " Manage plugins for apps built with Visual Studio Tools for Apache Cordova"
services: "na"
author: "jmatthiesen"
ms.prod: "visual-studio-dev15"
ms.devlang: "javascript"
ms.tgt_pltfrm: "mobile-multiple"
ms.workload: "na"
ms.date: "09/10/2015"
ms.author: jomatthi
--- 

# Manage plugins for apps built with Visual Studio Tools for Apache Cordova

Apache Cordova uses plugins to provide access to native device capabilities that aren’t available to simple web apps, such as access to the file system. A plugin is a cross-platform Cordova library that accesses native code and device capabilities through a JavaScript interface. When required, the plugin also updates the platform manifest to enable device capabilities. Not all plugins are supported or needed on all device platforms.

You enable plugins by using the Cordova config.xml file. Visual Studio provides ways to update this file using the configuration designer.

## <a id="Adding"></a>Add a core plugin

You can add a core Cordova plugin by using Visual Studio. When you build your solution, the plugin is installed from the Cordova registry.

1. In **Solution Explorer**, open the shortcut menu for the config.xml file, and then choose **Open** or **View Designer**.

2. In the configuration designer, choose the **Plugins** tab, and then choose the **Core** page.

3. Select a plugin, and then choose the **Add** button.

    ![Adding a plugin](media/core-plugins/IC795804.png)

    When you add the plugin, Visual Studio adds an element to your **config.xml** file.

    To write code for a particular plugin, see the [Plugin APIs](http://cordova.apache.org/docs/en/4.0.0/cordova_plugins_pluginapis.md.html#Plugin%20APIs).

## <a id="Custom"></a>Add a custom plugin

1. In **Solution Explorer**, open the shortcut menu for the config.xml file, and then choose **Open** or **View Designer**.

2. In the configuration designer, choose the **Plugins** tab, and then choose the **Custom** page.

3. Specify **Plugin ID**, **Local** or **Git** as the source, and then provide the location by entering the npm package name (the id), browsing to a local folder, or specifying a Git repository (ex. [https://github.com/cordova-sms/cordova-sms-plugin](https://github.com/cordova-sms/cordova-sms-plugin)).

    ![Cordova_Plugin_Custom](media/core-plugins/IC795805.png)

     Visual Studio adds a plugin folder for that custom plugin. The important plugin files include: **plugin.xml**, the plugin’s **src** folder and **www** folder.

## <a id="removing"></a>Remove a plugin

1. In **Solution Explorer**, open the shortcut menu for the config.xml file, and then choose **Open** or **View Designer**.

2. In the configuration designer, choose the **Plugins** tab, and then choose the **Installed** page.

3. Choose a plugin, and then choose the **Remove** button.

    ![Removing a plugin](media/core-plugins/remove-plugins.png)

    If you experience any errors when you add remove a plugin, see these [tips and workarounds](../tips-workarounds/general-tips.md).

## <a id="Updating"></a>Update a plugin to use the latest version

Use the configuration designer to update a plugin to a newer version. The configuration designer always adds the most recent version of a plugin to your project when it is installed.

To update, simply [remove the plugin](#removing) and then [Add it again](#Adding).

The [Cordova plugins registry](http://plugins.cordova.io) provides information about different plugin versions.

## <a id="Older"></a>Update a plugin to use an older version

[Remove the plugin](#removing), and then add the appropriate version of the plugin by directly editing **Config.xml** file of your project.

The [Cordova plugins registry](http://plugins.cordova.io) provides information about different plugin versions.

## <a id="Custom"></a>Extend a custom plugin

At times, the custom plugins in the Cordova registry might not meet all your app requirements, and you might want to extend a plugin or create your own plugin. For example, if you need to offload computationally expensive functions to native code, expose new device capabilities to your app, or apply a fix to an existing plugin that you would prefer not to release publicly, you might want to extend or create a plugin. You can find more information about creating your own plugins in the [plugin development guide](https://go.microsoft.com/fwlink/p/?LinkID=510633) in the Cordova documentation.

If you need to extend your app using a custom plugin, check the plugin registry first and use code that others have already written. If an existing plugin is close to what you need, download it, make improvements, and then submit those changes to the original author. This is a great way of giving back to the Cordova community and making it easier for others to solve similar problems. Install the custom plugin using the configuration designer. When the plugin.xml file is next to the www folder in the project folder tree, the required JavaScript files from the plugin’s www folder will be loaded automatically at runtime. You do not need to reference these files from an HTML file. You can also set breakpoints within these code files if needed. The build process also compiles any platform-specific files in the src folder.

## See Also
[Cordova config.xml documentation](https://go.microsoft.com/fwlink/p/?LinkID=510632)
[Cordova plugins registry](http://plugins.cordova.io)
