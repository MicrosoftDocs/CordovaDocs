---
title: "Use Apache Cordova with Visual Studio Team Services"
description: "Use Visual Studio Tools for Apache Cordova with Visual Studio Team Services"
author: "johnwargo"
ms.technology: "cordova"
ms.prod: "visual-studio-dev15"
ms.devlang: "javascript"
ms.tgt_pltfrm: "mobile-multiple"
ms.workload: "na"
ms.date: "01/07/2020"
ms.author: jowargo
---

# Use Visual Studio Tools for Apache Cordova with Visual Studio Team Services

Visual Studio Tools for Apache Cordova (TACO) is designed to work with a number of different team build systems. Since the Cordova projects TACO creates are standard Apache Cordova projects built using the [Cordova Command Line interface (CLI)](https://go.microsoft.com/fwlink/?LinkID=533773), any project created using TACO will work with any Cordova compatible solution.

The [Microsoft Visual Studio Team Services (VSTS) Agent](https://github.com/Microsoft/vsts-agent/blob/master/README.md) enables continuous integration (CI) on any platform. This specific agent provides a build and release agent for Linux and macOS.

Visual Studio Team Services can take advantage of a pre-built Cordova tasks that add additional features to streamline setup like managing certificates for iOS. Simply install the **[Cordova Build](https://go.microsoft.com/fwlink/?LinkID=691188)** agent and add a Cordova Build task to your build definition.

<table style="width: 100%; border-style: none;"><tr>
<td style="width: 140px; text-align: center;"><img src="https://raw.githubusercontent.com/Microsoft/vsts-cordova-tasks/master/docs/media/misc/cordova_logo_white_purple.png" alt="Apache Cordova logo" /></td>
<td><strong>Cordova Build</strong><br />
Visual Studio Client Tools<br />
<i>Streamline CI setup for your Apache Cordova, PhoneGap, Ionic, or Cordova CLI compatible app using a set of useful pre-defined build steps.</i><br />
<a href="https://go.microsoft.com/fwlink/?LinkID=691188">Install now!</a>
</td>
</tr></table>

## More Information

+	[Learn about other CI options](ci-guide.md)
