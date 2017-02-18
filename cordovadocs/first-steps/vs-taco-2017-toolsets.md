<properties
   pageTitle="Visual Studio Tools for Apache Cordova: Toolsets | Cordova"
   description="Using Toolsets in Visual Studio Tools for Apache Cordova"
   services="na"
   documentationCenter=""
   authors="johnwargo"
   tags=""/>
<tags ms.technology="cordova" ms.prod="visual-studio-dev15"
   ms.service="na"
   ms.devlang="javascript"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="na"
   ms.date="11/04/2016"
   ms.author="johnwargo"/>

# Visual Studio Tools for Apache Cordova: Toolsets

The Apache Cordova development toolchain consists of a variety of tools, the Cordova CLI, NodeJS, Node Package Manager (npm), and the mobile platform native SDKs. Each version of Apache Cordova works with a specific minimum versions of each tool. In order to simplify management of Visual Studio's Cordova development environment, Visual Studio Tools for Apache Cordova packages the required tool versions for each Cordova platform version into **Toolsets**. 

This approach enables a Visual Studio instance to be configured with multiple Cordova toolsets, and switch between them on a project by project basis as needed. For example, developers can maintain an existing application using an older Cordova toolset, but switch to a later Cordova version for a new application.

Toolsets are built and maintained by Microsoft, and deployed into Visual Studio using the Visual Studio Installer. When the Cordova development team releases a new Toolset, you'll use the Visual Studio Installer to update your environment to the latest version.  Once you install a newer Toolset, the Visual Studio Installer will list older Toolsets in the **Individual Components** list. 

## Selecting A Toolset  

The toolset used for a particular Cordova application project is controlled through an option on the **Toolset** tab in the Cordova configuration editor: 

![Apache Cordova Configuration Editor: Local Cordova Configuration](media/vs-taco-2017-toolsets/figure-01.png) 

In this example, the latest version of Apache Cordova is selected, and the pane lists the associated tool versions deployed into the development environment and used to build, test, and deploy Cordova applications. The **Toolset Name** dropdown list will show each installed toolset available to Visual Studio. 

> **Note:** If you're looking for a particular Cordova version and it does not appear in the list, you must launch the Visual Studio Installer to install the required toolset.
 
A project's toolset is stored in a custom property in the project's `config.xml` file. If you're editing the file manually rather than using the configuration editor, you'll find the setting in the following property: 

```xml
<vs:toolsetVersion>6.3.1</vs:toolsetVersion>
``` 

## Using An External Cordova Development Environment

If you have an existing Cordova development environment installed external to Visual Studio that you want to use, select the **Global Cordova x.x.x** option shown in the following figure:  

![Apache Cordova Configuration Editor: Global Cordova Configuration](media/vs-taco-2017-toolsets/figure-02.png)

With this option selected, Visual Studio will make calls into the globally installed Cordova tools as needed instead of using any toolsets it has installed internally.
