<properties pageTitle="Detect security issues using Intune, Active Directory, and Azure"
  description="Detect security policy and compliance violations using Intune MAM/MDM, Active Directory, and Azure."
  services=""
  documentationCenter=""
  authors="clantz" />

# Detect potential security threats 
Security is a very broad topic that covers a number of different aspects of an app's lifecycle. Securing an app often represents a number of tradeoffs and key decisions and even the most carefully crafted app can have unexpected security gaps. Preventing security issues really comes down to following guidence to reduce risk and using tools that help you identify problems before your app has even shipped. 

For the most part you should apply the same [best practices to your code as you do for web apps](https://code.google.com/archive/p/browsersec/wikis/Main.wiki) along with taking advantage of [Cordova platform security features](./cordova-security-platform.md), properly [authorizing your users](./cordova-security-auth.md), and taking steps to secure [data you store locally or send to or from services](./cordova-security-data.md). However, here are some additional things you can do to further reduce risk.

##Find issues with linting & code analysis tools
Static and dynamic code analysis tools can also help you identify problems and before you release your app. Linting/hinting tools like [JSHint](http://jshint.com/) for JavaScript, [FxCop](https://msdn.microsoft.com/en-us/library/bb429476.aspx) for C#, [OCLint](http://oclint.org/) for Objective-C (iOS), or the [Android SDK's lint](http://developer.android.com/tools/debugging/improving-w-lint.html) tool can help get you going. 

You can then step into more advanced static and dynamic code analysis tools like those offered by [HP Fortify](http://www8.hp.com/us/en/software-solutions/application-security/). Products like these are particularly useful if you are subject to compliance rules like PCI DSS.

JavaScript code typically makes up the bulk of your app's code and JSHint/JSLint or more advanced code analysis tools can be run on your JavaScript code directly (say via a [Gulp task](https://www.npmjs.com/package/gulp-jshint)). This makes it easy to add to any [Continous Integration](http://go.microsoft.com/fwlink/?LinkID=691186) you may be doing. 

C#, Objective-C, Java, and C++ code will normally be in plugins or the Cordova implementations for platforms like Android and iOS. You can get the most complete coverage of this type of code and then running a build for the platform in question. A fully formed project is present in the "platforms" folder in the project (ex: platforms/android).

##Device compliance, malware, and jailbreak detection with Intune
When building an internal facing app, Mobile Device Management (MDM) and Mobile Application Management (MAM) solutions like **[Microsoft Intune](https://www.microsoft.com/en-us/server-cloud/products/microsoft-intune/)** can detect Malware on Android and report jailbroken or rooted devices for iOS and Android. Intune's MAM capabilities can also be used stand alone and complement an existing MDM solution.

See the following articles for additional information:

- [Manage device compliance policies for Microsoft Intune](https://technet.microsoft.com/en-us/library/dn705843.aspx)
- [Create and deploy mobile app management policies with Microsoft Intune](https://technet.microsoft.com/en-us/library/mt627829.aspx)
- [Decide how to prepare apps for mobile application management with Microsoft Intune](https://technet.microsoft.com/en-us/library/mt631425.aspx). 

Intune provides two solutions for enabling its MAM features for Android and iOS devices: an app wrapping tool and an app SDK. Both can be used on an Android or iOS app to light up certain capabilities like limiting cut-copy-paste while the app is running, forcing a PIN, or forcing encryption. The Intune App SDK for Cordova is exposed via a Cordova plugin.  Adding the plugin is easy. 


1. In Visual Studio, when using Tools for Apache Cordova **Update 9 and up**, double click config.xml to go into config.xml designer, click on the **Plugins** tab, then **Custom**, select **ID** as the method to retrieve the plugin, and enter **cordova-plugin-ms-intune-mam**. 

    For earlier versions of Tools for Apache Cordova, right click on config.xml, select View Code, and then add one of the following. The plugin will be added on next build. 

    ```
    <plugin name="cordova-plugin-ms-intune-mam" spec="~1.0.0" />
    ```
    
    ...or for Cordova < 5.1.1...

    ```
    <vs:plugin name="cordova-plugin-ms-intune-mam" version="1.0.0" />
    ```

2. When using the command line or Visual Studio Code, you can add the plugin using the Cordova CLI as follows:

    ```
    cordova plugin add cordova-plugin-ms-intune-mam --save
    ```

See **[Intune's Cordova documentation](https://github.com/msintuneappsdk/cordova-plugin-ms-intune-mam)** for more information or if you would prefer to use the app wrapping tool, see Intune's documentation on the [Android](https://technet.microsoft.com/en-us/library/mt147413.aspx) and [iOS](https://technet.microsoft.com/en-us/library/dn878028.aspx) versions of the tools for more information.

##Threat detection
Even the most careful implementation can still have vulnerabilities so another aspect of security is detecting problems. In addition to the features Intune provides, Microsoft has a number of additional products that can help out in this space.

**[Azure Active Directory Identity Protection](https://azure.microsoft.com/en-us/documentation/articles/active-directory-identityprotection/)** is an extremely useful service that can help detect anomalous activity against user accounts. When combined with an authentication solution that supports Azure AD like Azure Mobile Apps or the ADAL Cordova plugin and passing the authentication token to any service calls you make for verification on the server side, you will be able to gain deep insights into potential brute force or compromised logins attacks.

See [Azure Active Directory Identity Protection documentation](https://azure.microsoft.com/en-us/documentation/articles/active-directory-identityprotection/) along with the articles on [authenticating users](./cordova-security-auth.md) and [securing data at rest and over the wire](./cordova-security-data.md) for additional information.

For Azure based services, databases, and VMs, you can use **[Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/)** which builds on top of Azure's world class certifications and enables some powerful security features for your Virtual Machines and SQL databases including full support for auditing SQL servers, threat detection, transparent data encryption, and monitoring and notification about recommended updates.

**[Microsoft Advanced Threat Analytics](https://www.microsoft.com/en-us/server-cloud/products/advanced-threat-analytics/)** is an on-premisems.product that provides an intelligent and adapting mechanism for [detecting potential threats across services and servers in a datacenter](https://technet.microsoft.com/en-us/library/dn707706.aspx). This is particularly useful in the mobile context when your app needs to access services housed within a corporate datacenter rather than Azure. 

##Additional Security Topics
- [Learn about Cordova platform and app security features](./cordova-security-platform.md)
- [Encrypt your local app data](./cordova-security-data.md)
- [Learn about securely transmitting data](./cordova-security-xmit.md)
- [Authenticate users with Azure Mobile Apps or the Active Directory Authentication Library for Cordova](./cordova-security-auth.md)
- [Quickly remediate security issues](./cordova-security-fix.md)
