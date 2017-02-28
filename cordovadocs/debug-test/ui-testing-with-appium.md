<properties pageTitle="UI Testing for Apache Cordova Apps with Appium"
  description="UI Testing for Apache Cordova Apps with Appium"
  services=""
  documentationCenter=""
  authors="Kraig Brockschmidt" />

#UI Testing for Apache Cordova Apps with Appium

UI testing is a core complement to unit testing (and also to integration testing, which is essentially unit testing across larger components in an app’s source code). Whereas unit testing exercises an app’s code directly by calling methods and accessing properties, revealing low-level functional defects, UI testing exercises an app through its external user interface, just as customers will. This reveals defects in the user experience, such as layout problems on different devices, that unit testing cannot.

The goal of UI testing is to achieve 100% coverage of *usage scenarios*, including the use of real data, live services, and so forth (rather than using mocks as with unit testing). This means that a full suite of UI tests will visit every page in the app, interact with every control, and test flows between multiple pages. For example, UI testing will try using the app without authenticating, then it can test those same pages again after logging in with suitable credentials.
 
A full suite of UI tests often exercises both specific parts of the app and more comprehensive flows that reflect customer usage scenarios. Specific tests help isolate defects quickly when a tests fails, such as when a button is incorrectly enabled without having acceptable data in an entry field. This is similar to how specific unit tests and more comprehensive integration tests work together. You want your lowest-level unit tests to exercise a code unit with one set of arguments, so if the test fails you know exactly why. Once you know that you have coverage of those specific cases, you can build higher-level integration tests. With UI testing, then, you want to include low-level tests to catch defects early, without, of course, neglecting user scenarios in which you need to test the flow of data between multiple pages in the app.

This does not mean that you should create individual UI tests that, for example, check only whether one text control accepts input or not. Unless you want to restrict the type of input (like phone numbers), you’re more interested in whether the control is enabled at the appropriate times, and whether entered data flows to the next part of the UI properly. When you do restrict the type of data entered, you probably want to test that part of the UI, and test the appropriate visibility of any messages that indicate bad input.

As we’ll see, what you’re really checking for in a UI test is that a specific sequence of actions brings about a desired state change in the UI, which possibly involves multiple controls. We’ll cover this in the section “Designing and writing UI test cases.”

Before that, however, we need to set up a suitable UI testing environment. For apps written with JavaScript and Apache Cordova, a good choice is to use the UI testing tool called Appium. After checking a few prerequisites, then, we’ll walk through setting up Appium and learn how to run UI sequences. We’ll then see how to wrap those UI sequences within tests cases that assert the desired UI state change. We’ll then finish with how to debug UI tests, along with a brief look at using device farms for testing.

> **Note**: For the samples used in this tutorial, refer to the [ui-testing folder of the Cordova samples repository](https://github.com/Microsoft/cordova-samples/tree/master/ui-testing). Once you've created a local copy, run *npm install* to install everything referenced in package.json.

## Appium and Node.js

UI testing fundamentally requires the use of some agent on a mobile platform that is capable of programmatically driving the UI of an arbitrary app. On iOS and Windows this agent is called UI Automation; on Android it’s UIAutomator. 

The big drawback with these platform-specific tools is that they are, of course, specific to each platform. Practically speaking, this means you have to write separate tests for each, which negates much of the cost-savings you want to realize by using a cross-platform technology like Cordova.

Fortunately, others before you have faced this same situation and have come up with various cross-platform testing solutions. The one we’ll focus on here is called Appium, which is especially well-suited for Cordova apps because it allows you to write tests in any number of languages, including JavaScript (specifically Node.js JavaScript, just as we’ve already used with unit tests).

> **Note**: At the time of writing, Appium supports testing only iOS and Android apps.

To understand Appium it’s necessary to understand just a little about the standard WebDriver interface. WebDriver, to put it succinctly, is the HTTP API that modern browsers implement to allow external agents to programmatically drive, and thus also test, a web application. And because WebDriver is an HTTP API, it means you can write test code in any language that can generate the appropriate HTTP requests. (It also means the API is asynchronous, as we’ll see later on.)

As illustrated below, Appium builds on this idea by extending the WebDriver API to accommodate the nature of mobile apps. Technically speaking, Appium is a server process that listens for those extended WebDriver HTTP requests, then calls into the UI Automation or UI Automator APIs on the mobile device or an emulator. This means, again, that you can write tests in any language that can generate suitable HTTP requests.

![The Appium server translates WebDriver requests to UI automation calls](media/uitest/appium-server.png)
 
What’s significant with testing a Cordova app is that your tests need to reach inside the WebView control that's hosting your UI, which we’ll see shortly. Fortunately, those WebViews also understand the WebDriver interface, so Appium effectively reaches inside an app through the UI automation API to connect with a WebView, and then lets you drive that WebView for your testing purposes. 

It’s helpful to know from the start that you can work with Appium either through a GUI tool or through the command line. In this documentation we’ll primarily focus on using Appium via the command line as it’s best suited for test automation.

 