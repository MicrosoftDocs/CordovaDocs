<properties pageTitle="Unit testing environments for JavaScript"
  description="Unit testing environments for JavaScript"
  services=""
  documentationCenter=""
  authors="Kraig Brockschmidt" />

#Unit testing environments for JavaScript and Apache Cordova
Unit testing in an Apache Cordova app means writing tests that exercise the app’s JavaScript code, but this happens outside the context of the running app and outside the context of any given mobile platform. Where unit testing is concerned, that is, it’s actually not particularly relevant that Cordova is involved: the tests and the code under test are loaded and executed into a separate runtime. This has a practical implication about plug-ins and platform features that is dicussed in [Using mocks for platform APIs, plugins, and other external dependencies](unit-test-07-mocks.md). For now, what’s important is understanding how JavaScript code executes in response to tests.

A unit testing environment consists of the three components: the runtime, a test framework, and a test runner:

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
<tr>
<td>Component</td><td>Description</td><td>Examples for JavaScript>
</tr>
<tr>
<td>Runtime</td><td>Loads and executes code being tested outside the app. This can happen in a browser or a separate runtime sometimes referred to as a “headless browser.”</td><td>Browsers; Node.js, PhantomJS, Chrome V8</td>
</tr>
<tr>
<td>Test framework</td><td>Defines how to write pieces of code that are specifically identified as “tests,” typically in the same language as the code being tested so they can share the same runtime, but this is not required.</td><td>Jasmine, QUnit, Mocha along with libraries like Chai and assert.js</td>
</tr>
<tr>
<td>Test runner</td><td>Executes tests defined by a supported framework within a supported runtime, and produces test reports.</td><td>Chutzpah (uses PhantomJS), Karma (uses a browser)</td>

</tr>
</table>
<p>&nbsp;</p>

With these components in place, it’s now a matter of invoking the test runner at appropriate times and viewing the reports. This can be done manually or as part of an automated build process, and the test runner might be invoked through the command line or through integration with the Visual Studio IDE.

Some test runners, like Chutzpah, have a Visual Studio *test adapter* that integrates the tool and its reporting into Visual Studio’s Test Explorer. Other test runners, like Karma, don’t have a test adapter but can still be integrated into Visual Studio’s Task Runner Explorer in conjunction with tools like Grunt and gulp.

All of these relationships are illustrated below:

![Components of a unit testing environment](media/environments/01-components.png)

As noted in the [Primer](./primer.md), unit testing runs code inside a runtime that’s potentially different from the one that the running app will use on a mobile platform, which means there are potential behavioral differences. This can affect test debugging, as described in [Runtime variances](unit-test-06-debug.md#variances) in the debugging topic. It also makes it necessary to do special handling for unit code that calls platform APIs, as described in [Using mocks for platform APIs, plugins, and other external dependencies](unit-test-07-mocks.md).

Such differences can also affect the performance of the final app on the mobile platform. This is where a complete set of automated functional tests provides the next layer of validation. If those automated tests exercise all the app’s features that rely on the behaviors of your unit-tested code, then those test should catch any variations that might exist between the runtimes.
