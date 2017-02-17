<properties pageTitle=" Quickly remediate security issues using CodePush and Intune"
  description="Quickly fix and remediate security policy and compliance violations using CodePush and Intune MDM."
  services=""
  documentationCenter=""
  authors="clantz" />

# Quickly remediate security issues using CodePush and Intune
Security is a very broad topic that covers a number of different aspects of an app's lifecycle. Securing an app often represents a number of tradeoffs and key decisions and even the most carefully crafted app can have unexpected security gaps. When you do identify a threat or security issue in an app that has been released, fixing it quickly can be critical particularly if you need to adhere to strict compliance rules in a regulated industry. 
 
##CodePush
One of the more frustrating aspects of mobile apps particularly for those that come from a web based background is that there are delays associated with getting app updates in the hands of customers. These can be driven by either app store submission delays which can be up to 10 days for iOS or simply the fact that not every user actually updates their apps when prompted.

**[CodePush](http://microsoft.github.io/code-push/)** is service that is designed to resolve this problem for Android and iOS based devices. The service allows you to update local static content (HTML, CSS, images) and JavaScript code in your app without an app store submission. The service supports both Cordova and React Native based apps through the use of a plugin.  You can even wire it into continuous delivery flows via a [Visual Studio Team Services extension](https://marketplace.visualstudio.com/items?itemName=ms-vsclient.code-push).

In the context of security this is extremly useful for quickly remediating security vulnerabilities. Using the service you can force updates to end users (or prompt if you prefer) and ensure everyone is on the same version of the code with the fix applied. Any fixes you can do in JavaScript code can then be deployed without an app store submission which can drop a 10 day turnaround for iOS to minutes!

See the **[CodePush Getting Started Guide](http://microsoft.github.io/code-push/docs/getting-started.html)** for information on getting up and running.

##Use an interal app store
Internal facing mobile apps are becoming increasingly common and some organizations opt to deploy these to public app stores. The challenge with this approach is that updates to the native app container are then subject to app store turnaround times (though CodePush can help remediate concerns with static and JavaScript content).

Fortunately, Mobile Device Management (MDM) suites like **[Microsoft Intune](https://www.microsoft.com/en-us/server-cloud/products/microsoft-intune/)** typically provide an internal app store that enable internal users to quickly gain access to both internal and externally developed apps. These features also include bring-your-own-device (BYOD) scenarios where mobile users may not want their devices controlled as strictly as corporate owned ones. Intune can also reduce the scope of your risk by restricting access to apps based Active Directory user groups. This can be useful for "super user apps", development or test versions of apps, or when you simply want to only offer an app to a subset of employees.

As a result, Intune's internal app store allows you to reduce the amount of time it takes to make an app update available to your internal users from days to minutes and can reduce the scope of your risk. Intune's mobile app store / company portal capabilities include support for Android, iOS, Windows, and Samsung KNOX based devices.  

See **[deploy and configure apps with Microsoft Intune](https://technet.microsoft.com/en-us/library/dn646965.aspx)** for more details.

##Additional Security Topics
- [Learn about Cordova platform and app security features](./cordova-security-platform.md)
- [Encrypt your local app data](./cordova-security-data.md)
- [Learn about securely transmitting data](./cordova-security-xmit.md)
- [Authenticate users with Azure Mobile Apps or the Active Directory Authentication Library for Cordova](./cordova-security-auth.md)
- [Detect potential security threats](./cordova-security-detect.md)
