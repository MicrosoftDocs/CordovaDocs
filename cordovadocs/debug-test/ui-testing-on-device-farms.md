<properties pageTitle="Testing on device farms"
  description="Testing on device farms"
  services=""
  documentationCenter=""
  authors="Kraig Brockschmidt" />

# Testing on device farms

As a mobile developer, you’re probably already acutely aware of the need to test your app on many different devices. Although you can work with a few emulator configurations and physical devices directly, getting broad coverage of many device variations gets very cumbersome and expensive.

For this reason, several companies have created device farms to manage thousands of individual devices, which you then access through a web portal and through programmatic APIs. Here are a few of the most popular ones:

<style>
    table, th, td {
        border: 1px solid black;
        border-collapse: collapse;
    }
    th, td {
        padding: 5px;
    }
</style>
<table>
<thead>
<tr>
Name	Devices/Platforms	Appium Support	Notes
</tr>
</thead>
<tbody>
<tr>
<td>
<a href="https://saucelabs.com/">Sauce Labs</a>
</td>
<td>
Emulators, physical devices; Android, iOS
</td>
<td>
Yes, any language and test framework.
</td>
<td>
All pricing plans offer unlimited manual testing.
</td>
</tr>
<tr>
<td>
<a href="https://www.xamarin.com/test-cloud">Xamarin Test Cloud</a>
</td>
<td>
Physical devices; Android, iOS, Windows
</td>
<td>
In progress. Tests for Cordova apps are presently written using Calabash and Ruby.
</td>
<td>
Works for any app written with any toolset (that is, not limited to Xamarin apps).
</td>
</tr>
<tr>
<td>
<a href="https://aws.amazon.com/device-farm/">Amazon AWS Device Farm</a>
</td>
<td>
Physical devices; Android, iOS
</td>
<td>
Yes, Java and Python only.
</td>
<td>
Offers a pay-as-you-go per-minute plan.
</td>
</tr>
<tr>
<td>
<a href="http://testdroid.com/">TestDroid</a>
</td>
<td>
Physical devices Android, iOS
</td>
<td>
Yes, Java and Python only.
</td>
<td>
N/A
</td>
</tr>
</tbody>
</table>

Sauce Labs generally has the best support for Appium across the board (Sauce Labs is a backer of Appium), including the ability to specify your Sauce Labs username and access key directly in the Appium configuration alongside your desired platform and device values. For example, here is a configuration object for running tests on an Android Emulator on Sauce Labs:

	var config = {};

	config.android19Hybrid = {
	    automationName: 'Appium',
	    appiumVersion: '1.3',
	    browserName: '',
	    platformName: 'Android',
	    platformVersion: '4.4.2',
	    autoWebview: true,
	    deviceName: 'Android Emulator',
	    app: "sauce-storage:weather-android-debug.apk",
	    newCommandTimeout: timeouts.appium,
	};

	var appDriver = yiewd.remote({
	    hostname: 'ondemand.saucelabs.com',
	    port: 80,
	    username: '<YOUR_USERNAME_HERE>',
	    password: '<YOUR_ACCESSKEY_HERE>'
	});

Notice that the device name is specific, and that *app* must be either a publically-accessible URL or point to a package you’ve uploaded to [Sauce Labs’ temporary storage](https://wiki.saucelabs.com/display/DOCS/Uploading+Mobile+Applications+to+Sauce+Storage+for+Testing) (such as *sauce-storage:weather-android-debug.apk* shown here). You’ll need to replace the app value with a reference to your own app, of course, and you'll need to set the user name and access key for your own account with Sauce Labs.

Full guidance can be found on Sauce Labs’ [Mobile Application Testing page](https://wiki.saucelabs.com/display/DOCS/Mobile+Application+Testing).
