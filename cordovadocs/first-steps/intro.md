<properties
   pageTitle="Visual Studio Tools for Apache Cordova | Cordova"
   description="Visual Studio Tools for Apache Cordova"
   services="na"
   documentationCenter=""
   authors="normesta, johnwargo"
   tags=""/>
<tags ms.technology="cordova" ms.prod="visual-studio-dev15"
   ms.service="na"
   ms.devlang="javascript"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="na"
   ms.date="10/31/2016"
   ms.author="johnwargo"/>

# Visual Studio Tools for Apache Cordova

Microsoft's Visual Studio Tools for Apache Cordova is a Microsoft Visual Studio workload that simplifies development of mobile apps on Android, iOS, and Windows using [Apache Cordova](http://cordova.apache.org/). Apache Cordova enables cross-platform mobile development using standard web technologies (HTML, CSS, and JavaScript). Visual Studio Tools for Apache Cordova is a component of Microsoft's Tools for Apache Cordova (TACO) that includes the following:  

+ [Visual Studio Code Cordova Extension](https://marketplace.visualstudio.com/items?itemName=vsmobile.cordova-tools) an extension for Visual Studio Code that provides Visual Studio Code with IntelliSense, debugging, and build support for Apache Cordova and Ionic projects.
+ [remotebuild](https://www.npmjs.com/package/remotebuild) for Mac OS is node module that provides a secure, remote build server to build, run and debug iOS Apache Cordova apps from within Microsoft Visual Studio.
+ TACO CLI is a set of command line utilities designed to simplify hybrid mobile development using Apache Cordova. 
 
**Note:** Microsoft no longer actively maintains the TACO CLI, you can download the project's files from its [GitHub Repository](https://www.npmjs.com/package/taco-cli)

## Overview

Microsoft's Visual Studio Tools for Apache Cordova deploys as an workload to Microsoft Visual Studio, installed along with your initial Visual Studio installation, or added to it later by simply re-running the installer. The workload installs a complete Apache Cordova development environment within Visual Studio, plus adds Cordova project templates and Cordova-specific capabilities to the IDE. It includes the following capabilities:

+ Simplified installation of a complete Apache Cordova development environment.
+ Create and manage Apache Cordova application projects for Android, iOS, and Windows in Visual Studio.
+ Code Cordova application in HTML, CSS and JavaScript.
+ Add or remove Cordova plugins to/from their Cordova application projects.
+ Intellisense for Cordova APIs.
+ Test Cordova applications in the browser, emulators, simulators and physical devices.
+ Debug Cordova web application content using the Visual Studio debugger.
+ Support multiple versions of Apache Cordova.

The most unique capability is the tool's ability to deliver live debugging of Android, iOS, and Windows Cordova applications, all from within the IDE. Use the Visual Studio debugger to attach to iOS, Android, and Windows apps, hit breakpoints, and inspect code using the console and DOM Explorer. Support for iOS development is provided through remotebuild, a node module that runs on a remote Apple Mac development system.

The Visual Studio installer identifies and installs the right versions of the required SDKs, tools, and libraries that you need to build [Apache Cordova](http://cordova.apache.org/) applications using Visual Studio.

## Get the tools

Visual Studio Tools for Apache Cordova is installed using the standard Visual Studio installer which you can download from the [Microsoft Download Center](https://aka.ms/vs/15/release/vs_enterprise.exe). Refer to [Install Visual Studio Tools for Apache Cordova](vs-taco-2017-install.md) for complete installation instructions. Start writing your first Cordova app using [Create Your First App Using Visual Studio Tools for Apache Cordova](vs-taco-2017-first-app.md).

## Sample Applications

The ToDoList sample app shows how you can use different frameworks for your Cordova app. ToDoList allows users to create new tasks, check them off, and remove them. The app uses Microsoft Azure Mobile Services to store data, and also uses Bing Maps to provide valid addresses.

+ [AngularJS sample](http://go.microsoft.com/fwlink/p/?LinkID=398516)
+ [Backbone sample](http://go.microsoft.com/fwlink/p/?LinkID=398517)
+ [WinJS sample](http://go.microsoft.com/fwlink/p/?LinkID=398518)

>**Tip:** To download other samples for Visual Studio Tools for Apache Cordova that demonstrate multi-page navigation and other features, see the [Ionic SideMenu starter template](http://go.microsoft.com/fwlink/p/?LinkID=544745) and [WinJS Navigation template](http://go.microsoft.com/fwlink/p/?LinkID=544743) samples.
