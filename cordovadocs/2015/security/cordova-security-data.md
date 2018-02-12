<properties pageTitle="Securing Cordova app data using encryption and Intune"
  description="Secure Cordova local app data using encryption and the Intune MAM App SDK."
  services=""
  documentationCenter=""
  authors="clantz" />

# Encrypt your local app data
Security is a very broad topic that covers a number of different aspects of an app's lifecycle. Securing an app often represents a number of tradeoffs and key decisions. Like the web, Cordova is a very open platform and as a result it does not force you down a specific path that will always guarantee a secure app. Instead provides a set of tools that you can use to lock down your app as appropriate. 

For the most part you should apply the same [best practices to your code as you do for web apps](https://code.google.com/archive/p/browsersec/wikis/Main.wiki). Storing data locally is relatively straight forward with Cordova but securing it can be a bit more difficult. Generally using JavaScript based encryption schemes is a bad practice and [not considered secure](https://www.nccgroup.trust/us/about-us/newsroom-and-events/blog/2011/august/javascript-cryptography-considered-harmful/) and local and file storage are not encrypted. That said, features like [Apple's much discussed data protection](https://support.apple.com/en-us/HT202064) can be enabled by simply setting a PIN which products like Intune can force to happen for your app. However, and you may have multi-tenet requirements where data separation is required when multiple users access the same device or you would simply prefer to add an extra layer of app security by encrypting your local app data.

Here are some recommendations that can help encrypt sensitive data in Cordova apps. 

##Encrypt data using Web Crypto via Crosswalk and a shim
The best starting point whenever you are tackling a problem related to security is to rely on browser features as they undergo significant testing and have abundant real-world use going for them. While the API is a bit cumbersome, Web Crypto is a W3C standard that lets the browser itself encrypt data that has growing native browser support. 

Historically [crypto.subtle.encrypt and decrypt](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto) had [varying levels of support](http://caniuse.com/#search=web%20crypto) in browsers with one in particular being the biggest problem for Cordova: Android. To resolve this problem we'll need to take advantage of something called "Crosswalk."

> Note: If you would **prefer not to use Crosswalk** or find the Crypto API **too cumbersome**, there are some alternatives like Intel's excellent [com-intel-security-cordova-plugin](https://software.intel.com/en-us/app-security-api/api) Cordova plugin that are worth considering.


### Crosswalk
[Crosswalk](https://crosswalk-project.org/) is an open source project that allows you to use an updated version of the Chromium webview on Android devices. The [Crosswalk WebView Engine plugin](https://www.npmjs.com/package/cordova-plugin-crosswalk-webview) brings Android 4.0+ up to a recent version of Chromium including Web Crypto support. Adding it is the first step in enabling Web Crypto in your Cordova projects and the plugin is available in the config.xml designer's plugin tab.

See the article on **[improving Android browser consistency and features with the Crosswalk WebView](../develop-apps/cordova-crosswalk.md)** for additional details on setup including some important information on **emulator config**. 

Note that Crosswalk 14 can cause a crash when using Web Crypto and Crosswalk 16 has caused crashes in certain emulators. Crosswalk 15 appears to be a solid choice. If you run into unexpected crashes or odd behaviors, add this to config.xml (Right-Click &gt; View Code in VS):

```
<preference name="xwalkVersion" value="org.xwalk:xwalk_core_library:15+" />
```

You should only add this preference if you encounter issues as it disables useful features like "Shared Mode".

### webcrypto-shim
iOS supports crypto.subtle with a webkit prefix and IE 11 and up also support web crypto but with a slightly different syntax. Thankfully, an API shim like [webcrypto-shim](https://github.com/vibornoff/webcrypto-shim) can resolve these differences for you such that the combination of Crosswalk in the shim give you native encryption capabilities.

In general, this is your best starting point. If for some reason you cannot use Crosswalk or are looking for more holistic solutions, there are community plugins and other solutions that can help.  

### Example
Here is a small code sample that demonstrates using Web Crypto to encrypt a string on Android, iOS, and Windows after adding the **[cordova-plugin-crosswalk-webview](https://www.npmjs.com/package/cordova-plugin-crosswalk-webview)** plugin to your project and referencing **[webcrypto-shim.js](https://github.com/vibornoff/webcrypto-shim)** and **[promiz.js](https://github.com/Zolmeister/promiz)** in your HTML.

First, crypto.subtle.encrypt and decrypt uses ArrayBuffers rather than strings or other types, so for this example we'll take advantage of two useful functions from [Google Developers](https://developers.google.com/web/updates/2012/06/How-to-convert-ArrayBuffer-to-and-from-String) for type string conversions.

```javascript
function stringToArrayBuffer(str) {
    var buf = new ArrayBuffer(str.length*2); // 2 bytes for each char
    var bufView = new Uint16Array(buf);
    for (var i=0, strLen=str.length; i < strLen; i++) {
        bufView[i] = str.charCodeAt(i);
    }
    return buf;
};

function arrayBufferToString(buf) {
    return String.fromCharCode.apply(null, new Uint16Array(buf));
};
```

Next we will generate an encryption key and then encrypt a string using crypto.subtle.encrypt.

```javascript
var stringToEncrypt = "Hey! Encrypt me!";

// Setup AES-CBC encryption
var randomVals = new Uint8Array(16);
crypto.getRandomValues(randomVals);
var cryptoSubtleAlgo = { "name": "AES-CBC", "length": 256, "iv": randomVals };

// Generate a key, encrypt
crypto.subtle.generateKey(cryptoSubtleAlgo, true, ["encrypt", "decrypt"])
    .then(function (key) {
        return crypto.subtle.encrypt(cryptoSubtleAlgo, key, stringToArrayBuffer(stringToEncrypt))
    })
    .then(function (result) {
        // Log encrypted string
        console.log("Before: " + stringToEncrypt);
        console.log("After: " + arrayBufferToString(result));
    });
```

##Consider community plugins
In addition to the above base capabilities there are a number of community plugins that can be used to encrypt data locally. Microsoft does not directly support these plugins, so security focused organizations should be sure to run a static and or dynamic code analysis tool on the resulting project code (including these plugins) during any planned security audits. However, Intel maintains one of these plugins and the plugin author of cordova-sqlite-storage offers support contracts for those that are interested.

<style>
    table, th, td {
        border: 1px solid black;
        border-collapse: collapse;
    }
    th, td {
        padding: 5px;
    }
</style>
<table><thead>
<tr>
<td align="left"><strong>Topic</strong></td>
<td align="left"><strong>Plugin</strong></td>
<td align="left"><strong>Purpose</strong></td>
<td align="left"><strong>Platform Support</strong></td>
</tr>
</thead><tbody>
<tr>
<td align="left">Secure Data Storage, Secure HTTP</td>
<td align="left"><strong><a href="https://www.npmjs.com/package/com-intel-security-cordova-plugin">com-intel-security-cordova-plugin</a></strong></td>
<td align="left"><p>This plugin provides a robust set of interfaces for secure storage and transmission of data. Specifically:</p>
<ul>
<li>It provides a functions to secure data in memory and then optionally persist the data in local storage.</li>
<li>It provides a functions to securely transmit data via HTTP including support for certificate pinning.</li>
</ul>
<p>The plugin comes with <a href="https://software.intel.com/en-us/app-security-api/api">excellent documentation</a> and <a href="https://software.intel.com/en-us/node/604512">code snippets</a>.</p>
</td>
<td align="left">Android, iOS, Windows/Windows Phone 8.1, 10</td>
</tr>
<tr>
<td align="left">Token / Secret Storage</td>
<td align="left"><strong><a href="https://www.npmjs.com/package/cordova-plugin-secure-storage">cordova-plugin-secure-storage</a></strong></td>
<td align="left"><p>The plugin allows your application to securely store secrets such as auth tokens or encryption keys in native secure key/token stores.</p>
<p>Note that a side effect of this plugin is it forces users to set a PIN on Android devices (which could be good or bad depending on the situation) since this is required to enable the generation of cryptographic keys.</p>
</td>
<td align="left">Android, iOS</td>
</tr>
<tr>
<td align="left">Encrypted Data in a Database</td>
<td align="left"><strong><a href="https://www.npmjs.com/package/cordova-sqlite-storage">cordova-sqlite-storage</a></strong></td>
<td align="left"><p>The <strong><a href="http://html5doctor.com/introducing-web-sql-databases/">W3C WebSQL API</a></strong> is available on iOS and Android for storing data and can be combined with <strong>Web Crypto</strong> or <strong>com-intel-security-cordova-plugin</strong> to store encrypted values in a database. You can find a number of <a href="http://html5doctor.com/introducing-web-sql-databases/">tutorials</a> on WebSQL on the web.</p>
<p>However, WebSQL is limited to 50mb on iOS and the API is technically deprecated so it may not be available in future versions of device browsers. However, there are a set of plugins that use the same API to store data in your own <a href="http://sqlite.org/">SQLite</a> database without storage limits and add other features. The edition of this plugin you select will depend on your needs: </p>
<ul>
<li><a href="https://github.com/litehelpers/cordova-sqlite-storage">cordova-sqlite-storage</a> - Base version of the SQLite storage plugin with Android, iOS, and as of 1.4.0 both Windows/Windows Phone 8.1 and Windows 10 support. MIT licensed.</li>
</ul>
Other versions:
<ul>
<li><a href="https://github.com/litehelpers/cordova-sqlite-enterprise-free">cordova-sqlite-evfree</a> - An enhanced, enterprise focused version of the SQLite plugin targeted at enterprises with additional features. GPL v3 or commercial licensed. Install via its <a href="https://github.com/litehelpers/cordova-sqlite-enterprise-free.git">Git URI</a>.</li>
<li><a href="https://github.com/litehelpers/cordova-sqlcipher-adapter">cordova-sqlcipher-adapter</a> -  An <strong>alpha</strong> version of the cordova-sqlite-storage plugin that uses SQLCipher to <strong>encrypt all data</strong>. Install via its <a href="https://github.com/litehelpers/cordova-sqlcipher-adapter.git">Git URI</a>.</li>
<ul>
</td>
<td align="left">Android, iOS, Windows/Windows Phone 8.1, Windows 10</td>
</tr>
<!--
<tr>
<td align="left">Encrypted Database</td>
<td align="left">[In Development]<br /><strong><a href="https://github.com/litehelpers/cordova-sqlcipher-adapter">cordova-sqlcipher-adapter</a></strong></td>
<td align="left"<p>cordova-sqlcipher-adapter is a **release candidate** version of an enhanced adapter on top of the cordova-sqlite-storage plugin that uses SQLCipher to encrypt all data stored in a local database. You will need to install this version of the plugin using the <strong>Git URI</strong>: https://github.com/litehelpers/cordova-sqlcipher-adapter.git</p></td>
<td align="left">Android, iOS</td>
</tr>
-->
</tbody></table>

### Adding the plugins

1. In Visual Studio, when using Tools for Apache Cordova **Update 9 and up**, double click config.xml to go into config.xml designer, click on the **Plugins** tab, then **Custom**, select **ID** as the method to retrieve the plugin, and enter the plugin ID above (ex: cordova-plugin-secure-storage). 

    For earlier versions of Tools for Apache Cordova, right click on config.xml, select View Code, and then add one of the following depending on whether or not a Git URI needs to be used. The plugin will be added on next build.

    ```
    <plugin name="cordova-sqlite-storage" spec="~1.4.0" />
    <plugin name="io.litehelpers.cordova.sqlcipher" src="https://github.com/litehelpers/Cordova-sqlcipher-adapter.git" version="0.1.4-rc" />
    ```

    ...or for Cordova < 5.1.1...

    ```
    <vs:plugin name="cordova-sqlite-storage" version="1.4.0" />
    <vs:plugin name="io.litehelpers.cordova.sqlcipher" src="https://github.com/litehelpers/Cordova-sqlcipher-adapter.git" version="0.1.4-rc" />
    ```

2. When using the command line or Visual Studio Code, you can add the plugin using the Cordova CLI as follows:

    ```
    cordova plugin add cordova-sqlite-storage --save
    cordova plugin add https://github.com/litehelpers/Cordova-sqlcipher-adapter.git --save
    ```

##Consider Intune MAM features to force encryption
[Microsoft Intune](https://www.microsoft.com/en-us/server-cloud/products/microsoft-intune/) is a [mobile application management](https://en.wikipedia.org/wiki/Mobile_application_management) (MAM) and [mobile device management](https://en.wikipedia.org/wiki/Mobile_device_management) (MDM) platform that supports Android, iOS, and Windows devices. Intune's MAM capabilities can be used without managing devices which means it can be used in combination with existing MDM solutions like Airwatch and Mobile Iron. Currently it is targeted at Active Directory authorized apps and thus is most applicable to enterprise focused scenarios. It provides the ability to enforce policies at the **app level** including encryption of all local data. It's therefore a low friction way to increase your security. 

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

##Consider native Windows APIs for Windows
One often missed feature that the Windows platform for Cordova has is the ability to call **any** JavaScript enabled [Windows API](https://msdn.microsoft.com/en-us/library/windows/apps/br211377.aspx) from your Cordova app **without a plugin**. Many plugins for the Windows platforms are simple JavaScript adapters to conform to the plugin interface spec. 

This includes all features contained within the **Windows.Security** and **Windows.Security.Cryptography** namespaces! However, be aware that there may be some variations between Windows 10 and Windows 8.1 APIs depending on which OS you are targeting.

 See **[Windows API documentation](https://msdn.microsoft.com/en-us/library/windows/apps/br211377.aspx)** for additional details and specifics.

##Additional Security Topics
- [Learn about Cordova platform and app security features](./cordova-security-platform.md)
- [Learn about securely transmitting data](./cordova-security-xmit.md)
- [Authenticate users with Azure Mobile Apps or the Active Directory Authentication Library for Cordova](./cordova-security-auth.md)
- [Detect potential security threats](./cordova-security-detect.md)
- [Quickly remediate security issues](./cordova-security-fix.md)
