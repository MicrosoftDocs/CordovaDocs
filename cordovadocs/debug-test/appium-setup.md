<properties pageTitle="Appium setup"
  description="Appium setup"
  services=""
  documentationCenter=""
  authors="Kraig Brockschmidt" />

#Appium setup

There are two ways to install and run an Appium server, via npm on the command line, and by downloading and installing the Appium GUI tool.

Because our focus in this documentation is using Appium for test automation, we’ll be working with Appium from the command line and through code. We'll also install Appium using npm. If you want to try the GUI tool, you can install it from from [https://bitbucket.org/appium/appium.app/downloads](https://bitbucket.org/appium/appium.app/downloads) (This is linked from the Appium homepage, [http://appium.io/](http://appium.io/) so don’t worry about the bitbucket.org domain name.)

##Installing Appium via npm

Open a command prompt and install Appium globally via **npm**, as is common with development tools. This installs quite a few dependencies and might take a while depending on your Internet connection:

    npm install -g appium

The [Appium page on npmjs.org]( https://www.npmjs.com/package/appium) recommends also installing and running **appium-doctor** to check dependencies and environment variables:

    npm install -g appium-doctor
    appium-doctor --android  #Works on any Android-capable dev machine
    appium-doctor --ios      #Works only on a Mac, of course
    
Output for Android looks like the following; if anything is missing, you’ll see appropriate errors:

    info AppiumDoctor ### Diagnostic starting ###
    info AppiumDoctor  ✔ ANDROID_HOME is set to: d:\android\sdk
    info AppiumDoctor  ✔ JAVA_HOME is set to: C:\Program Files (x86)\Java\jdk1.7.0_55
    info AppiumDoctor  ✔ adb exists at: d:\android\sdk\platform-tools\adb.exe
    info AppiumDoctor  ✔ android exists at: d:\android\sdk\tools\android.bat
    info AppiumDoctor  ✔ emulator exists at: d:\android\sdk\tools\emulator.exe
    info AppiumDoctor ### Diagnostic completed, no fix needed. ###
    info AppiumDoctor
    info AppiumDoctor Everything looks good, bye!
    info AppiumDoctor
    
##Starting the Appium server

Once Appium is installed, open a new command prompt and start the server as below (with Appium installed globally as in the previous section, you can be in any folder):

    appium

The Appium server will stay running in this command window until you stop the server with Ctrl+C or close the window. (You can restart the server, of course, by just running *appium* again.) Typically, you’ll have one command window open with the server running, and use a separate one to run test from the command line.

In any case, once the server is started you’ll see output like the following, which tells you that it’s listening for WebDriver API requests on the indicated port:

    [Appium] Welcome to Appium v1.5.1
    [Appium] Appium REST http interface listener started on 0.0.0.0:4723

If you want to have the server listen to requests on a different port, such as localhost:4723, simply add a few arguments:

    appium --address 127.0.0.1 --port 4732

You can use other [Appium server arguments]( http://appium.io/slate/en/master/?javascript#appium-server-arguments) to also specify the environment configuration and point to the app package (which is what the GUI tool uses when it launches the server). For our purposes here, however, we’ll do all the configuration in code as described in the next section because we can then keep the configuration directly with the tests that depend on it.

##Connect to and configure the Appium server in code (test01.js)

Configuring the Appium server means settings the appropriate capabilities for the desired test environment along with pointing to the app package to test. Again, you can do this through the command line’s --default-capabilities arguments, as described on [The --defaultCapabilities flag]( http://appium.io/slate/en/master/?javascript#server-flags), or in code by initializing Appium with the options described on [Appium server capabilities]( http://appium.io/slate/en/master/?javascript#appium-server-capabilities).

The capabilities that we’re particularly concerned with here are as follows:

- **automationName**: “Appium” (the default) for native apps on iOS and Android API levels 17 and higher; “Selendroid” is used for native apps on Android API levels 16 and below. The documentation also states that this should be used also for hybrid apps, although “Appium” works fine with Cordova apps.
- **platformName**: “Android” or “iOS”
- **platformVersion**: the mobile OS version. Use a quoted string such as “7.1” or “4.4” for version numbers; on Android you can also use an integer API level value, such as 19.
- **app**: the local path or remote URL to the app package, either an .ipa (iOS), .apk (Android), or a .zip file with either. 
- **browserName**: when app is set, which must be “” (blank).
- **deviceName**: Specifies the device to which to deploy the app. For iOS, it must be one of the devices listed by *instrumented –s devices*. For Android, although you must specify some non-blank value for this capability, Appium always uses the first device shown by the *adb devices* command. 
- **autoWebview**: if true, moves Appium directly into a WebView context, which is of course what we want for a Cordova app. (Note again that Appium presently supports only Android and iOS. A Cordova app running on Windows runs as a native JavaScript app without a WebView, so at what time Appium supports Windows apps you would not use this flag on that platform.)
 
If you’ve built the WeatherApp sample for Android as described in Prerequisites, you should have an .apk file in hand already. Because that sample targets Android API level 19, we need to configure Appium for the same. The below, which you can find in **[test01.js](https://github.com/Microsoft/cordova-samples/blob/master/ui-testing/test01.js)**, does exactly this. You’ll need to change the app path for your system, of course:

    var wd = require("wd");
    var appDriver = wd.remote({
        hostname: '127.0.0.1',
        port: 4723,
    })
    
    var config = {};
    
    config.android19Hybrid = { 
       automationName: 'Appium',
       browserName: '',
       platformName: 'Android', 
       platformVersion: 19,// API level integer, or a version string like '4.4.2'
       autoWebview: true,
       deviceName: 'any value; Appium uses the first device from *adb devices*',
       app: "D:\\g\\cordova-samples\\weather-app\\WeatherApp\\bin\\Android\\Debug\\android-debug.apk" 
    };

> **Note**: the *config* object is just a helpful way to manage different configurations that you might use in your testing. You could create a .js file with an object containing all your target configurations, for example, and then use that same file in many different app projects.
 
The final step is to send this configuration to the Appium server through the *init *method:

    appDriver.init(config.android19Hybrid);

With the Appium server running in a separate command window, run this code now to see Appium in action:

    node test01.js

Note that Appium always runs the app on the first device returned from the *adb.exe devices* command (Android device bridge), regardless of what you specify in the **deviceName** capability. That’s why the code above just has a comment to that effect.

In any case, switch over to the Appium server window and you’ll see a whole lot of output as the app gets launched. To highlight a few of the more interesting parts, you’ll first see the request that shows the configuration, followed by an indication that a session was started:

    [HTTP] --> POST /wd/hub/session {"desiredCapabilities":{"automationName":"Appium", "browserName":"","platformName":"Android","platformVersion":19,"autoWebview":true, "deviceName":"any value; Appium uses the first device from *adb devices*", "app": "D:\\g\\cordova-samples\\weather-app\\WeatherApp\\bin\\Android\\Debug\\android-debug.apk"}}
    [MJSONWP] Calling AppiumDriver.createSession() with args: [{"automationName":"Appium", "browserName":"","platformName":"Android","platformVersion":19,"autoWebview":true, "deviceName":"any value; Appium uses ...
    [Appium] Creating new AndroidDriver session
    [Appium] Capabilities:
    [Appium]   automationName: 'Appium'
    [Appium]   browserName: ''
    [Appium]   platformName: 'Android'
    [Appium]   platformVersion: 19
    [Appium]   autoWebview: true
    [Appium]   deviceName: 'any value; Appium uses the first device from *adb devices*'
    [Appium]   app: 'D:\\g\\cordova-samples\\weather-app\\WeatherApp\\bin\\Android\\Debug\\android-debug.apk'
    [BaseDriver] Session created with session id: 6871e78b-f4ad-4cda-a74e-2d33817c67a5
    
You’ll then see the server locating and selecting a device using adb.exe (you actually see adb output quite a bit). In the output below, the IP address and port combination is the default for the Visual Studio Emulator for Android (a different emulator would likely have a different address and port):

    [ADB] Checking whether adb is present
    [ADB] Using adb.exe from d:\android\sdk\platform-tools\adb.exe
    [AndroidDriver] Retrieving device list
    [debug] [ADB] Trying to find a connected android device
    [debug] [ADB] Getting connected devices...
    [debug] [ADB] 2 device(s) connected
    [AndroidDriver] Using device: 169.254.109.177:5555

Appium removes the app if it exists on the device, then deploys the app again. Eventually it starts the Android UIAutomator on the device:

    [debug] [UiAutomator] Starting UiAutomator
    and then launches the app, finding its main activity:
    [debug] [ADB] Running d:\android\sdk\platform-tools\adb.exe with args:
    ["-P",5037,"-s","169.254.109.177:5555","shell","am","start","-n", "io.cordova.myapp70a2a3/io.cordova.myapp70a2a3.MainActivity", "-S","-a", "android.intent.action.MAIN","-c","android.intent.category.LAUNCHER","-f","0x10200000"]
    [debug] [ADB] Waiting for pkg: 'io.cordova.myapp70a2a3' and activity: 'io.cordova.myapp70a2a3.MainActivity' to be focused
    [debug] [ADB] Possible activities, to be checked: io.cordova.myapp70a2a3.MainActivity, .MainActivity, .io.cordova.myapp70a2a3.MainActivity
    [debug] [ADB] Getting focused package and activity
    [debug] [ADB] Found package: 'io.cordova.myapp70a2a3' and activity: '.MainActivity'

Then, because we set the **autoWebview** capability to true, Appium looks for the Webview in the app:

    [AndroidDriver] Setting auto webview
    [debug] [AndroidDriver] Getting a list of available webviews
    [debug] [AndroidDriver] WEBVIEW_11084 mapped to pid 11084
    [debug] [AndroidDriver] Getting process name for webview
    [debug] [AndroidDriver] Parsed pid: 11084 pkg: io.cordova.myapp70a2a3!
    [debug] [AndroidDriver] returning process name: io.cordova.myapp70a2a3
    [debug] [AndroidDriver] Found webviews: ["WEBVIEW_io.cordova.myapp70a2a3"]
    [debug] [AndroidDriver] Available contexts: ["NATIVE_APP","WEBVIEW_io.cordova.myapp70a2a3"]
    [debug] [AndroidDriver] Connecting to chrome-backed webview context 'WEBVIEW_io.cordova.myapp70a2a3'

The upshot of all this is that the app is running on the device, UIAutomator has started, and it’s been hooked up to the Webview in the app. At this point node.js exits back to the command prompt because there’s no more code in [test01.js](https://github.com/Microsoft/cordova-samples/blob/master/ui-testing/test01.js) to do anything with the app. (Again, Cordova apps running on Windows do not use a WebView because the platform supports JavaScript natively. However, Appium doesn’t at present support testing Windows apps, so this is not yet an issue.)

After 60 seconds the Appium server ends the session and stops the app automatically (you can change the timeout with the **newCommandTimeout** capability if desired). The Appium server, however, will still be running in its own command window, so you can continue to use it for subsequent tests without having to restart it, as we’re now ready to do.
