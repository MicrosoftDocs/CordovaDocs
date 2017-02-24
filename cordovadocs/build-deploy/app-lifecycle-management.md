<properties
   pageTitle="Application Lifecycle Management (ALM) with Apache Cordova Apps"
   description="Application Lifecycle Management (ALM) with Apache Cordova Apps"
   services="na"
   documentationCenter=""
   authors="kraigb, johnwargo"
   tags=""/>
<tags 
	ms.technology="cordova" 
	ms.prod="visual-studio-dev15"
   	ms.service="na"
   	ms.devlang="javascript"
   	ms.topic="article"
   	ms.tgt_pltfrm="mobile-multiple"
   	ms.workload="na"
   	ms.date="02/12/2017"
   	ms.author="johnwargo"/>

# Application Lifecycle Management (ALM) with Apache Cordova Apps

Developing apps for modern platforms involves many more activities than just writing code. DevOps (a combination of Development and Pperations) recognizes a variety of activities across an app’s complete lifecycle. These include Agile planning and tracking work, architecture, designing and implementing code, managing a source code repositories, running builds, managing continuous integration and deployment tasks, testing (including unit tests, UI, and performance tests), running various forms of diagnostics in both development and production environments, and monitoring app performance and user behaviors in real time through telemetry and analytics.

Visual Studio and Visual Studio Team Services (VSTS) provide a variety of DevOps capabilities (also referred to as application lifecycle management or ALM) for development organizations, a number of which are applicable to Cordova apps. Tools that are designed for .NET languages like C#, however, do not apply to JavaScript code. Other tools require tight integration with build and runtime environments. Because Cordova apps on Windows run as native apps, you’re able to use a variety of Visual Studio’s diagnostic tools such as performance profilers that are not available for non-Windows platforms.

The table below identifies the Visual Studio ALM features you can utilize with an Apache Cordova project, and which ones have limitations. Refer to the linked documentation for details on the features themselves.

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
    <tbody>
        <tr>
            <th>
                <p>Area</p>
            </th>
            <th>
                <p>Feature</p>
            </th>
            <th>
                <p>Supported with Apache Cordova</p>
            </th>
            <th>
                <p>Comments</p>
            </th>
        </tr>
        <tr>
            <td rowspan="5">
                <p>
                    <strong><a href="https://msdn.microsoft.com/library/dd286619.aspx">Agile tools</a>                      
                    </strong><br /> (using Visual Studio Team Services, including Team Explorer Everywhere)</p>
            </td>
            <td>
                <p>Manage backlogs and Sprints</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td rowspan="5">
                <p>All planning and tracking features are independent of project type and coding languages.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Work Tracking</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Teamroom Collaboration</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Kanban Boards</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Report and Visualize Progress</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
        </tr>
        <tr>
            <td rowspan="8">
                <p>
                    <strong><a href="https://msdn.microsoft.com/library/57b85fsc.aspx">Analyze and model your architecture</a>
                    </strong>
                </p>
            </td>
            <td>
                <p>Sequence Diagrams</p>
            </td>
            <td>
                <p>No</p>
            </td>
            <td rowspan="8">
                <p>Most design features rely on .NET and languages like C# and do not work with HTML, CSS, and JavaScript. See
                    <a href="https://msdn.microsoft.com/library/ff183189.aspx#modelingdiagramstools">Modeling Diagram Tools</a>                    for what aspects are related to code.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Dependency Graphs</p>
            </td>
            <td>
                <p>No</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Call Hierarchy</p>
            </td>
            <td>
                <p>No</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Class Designer</p>
            </td>
            <td>
                <p>No</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Architecture Explorer</p>
            </td>
            <td>
                <p>No</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>UML Diagrams (use case, activity, class, component, sequence, and DSL)</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Layer Diagrams</p>
            </td>
            <td>
                <p>No</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Layer Validation</p>
            </td>
            <td>
                <p>No</p>
            </td>
        </tr>
        <td rowspan="5">
            <p>
                <strong>Code</strong>
            </p>
        </td>
        <td>
            <p><a href="https://msdn.microsoft.com/library/ms181237.aspx">Use Team Foundation Version Control</a></p>
        </td>
        <td>
            <p>Yes</p>
        </td>
        <td>
            <p>

            </p>
        </td>
        </tr>
        <tr>
            <td>
                <p><a href="https://msdn.microsoft.com/library/hh850437.aspx">Getting started with Git in Team Services</a></p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p>

                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Code Analysis (references, suggested changes, etc.)</p>
            </td>
            <td>
                <p>No</p>
            </td>
            <td>
                <p>One exception is the Go To Definition command that works with JavaScript.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p><a href="https://msdn.microsoft.com/library/dn269218.aspx">Find code changes and other history with CodeLens</a></p>
            </td>
            <td>
                <p>No</p>
            </td>
            <td>
                <p>Not supported for JavaScript.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p><a href="https://msdn.microsoft.com/library/jj739835.aspx">Use code maps to debug your applications</a></p>
            </td>
            <td>
                <p>No</p>
            </td>
            <td>
                <p>Not supported for JavaScript.</p>
            </td>
        </tr>
        <tr>
            <td rowspan="6">
                <p>
                    <strong><a href="https://msdn.microsoft.com/library/ms181709.aspx">Build the application</a></strong>
                </p>
            </td>
            <td>
                <p>On-premises VSTS server</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p>Android, Windows, WP8, can be built on a Windows build server while a Mac can be used as a build server for
                    iOS. See <a href="http://go.microsoft.com/fwlink/?LinkID=691186">Build Apache Cordova Apps</a>
            </td>
        </tr>
        <tr>
            <td>
                <p>On-premises build server linked to Visual Studio Team Services</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p>See <a href="http://go.microsoft.com/fwlink/?LinkID=691186">Build Apache Cordova Apps</a> for instructions.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Hosted controller service of Visual Studio Team Services</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p>See <a href="http://go.microsoft.com/fwlink/?LinkID=691186">Build Apache Cordova Apps</a>.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>On-premises Jenkins CI or other build server</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p>See <a href="http://go.microsoft.com/fwlink/?LinkID=613703" target="_blank">Using Tools for Apache Cordova with the Jenkins CI System</a>                    for instructions.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Build definitions with pre- and post-scripts</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p>
                </p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Continuous integration including gated check-ins</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p>For VSTS, select "Continous Integration" under the "Triggers" tab of our Build Definition. See <a href="http://go.microsoft.com/fwlink/?LinkID=691186">Build Apache Cordova Apps</a>.
            </td>
        </tr>
        <tr>
            <td rowspan="6">
                <p>
                    <strong><a href="https://msdn.microsoft.com/library/ms182409.aspx">Testing the application</a></strong>
                </p>
            </td>
            <td>
                <p>Planning tests, creating test cases and organizing test suites</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p></p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Manual testing</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p></p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Test Manager (record and playback tests)</p>
            </td>
            <td>
                <p>Windows devices and Android emulators only</p>
            </td>
            <td>
                <p></p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Code coverage</p>
            </td>
            <td>
                <p>No</p>
            </td>
            <td>
                <p></p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Unit tests</p>
            </td>
            <td>
                <p>Yes, with third-party frameworks</p>
            </td>
            <td>
                <p>See <a href="../test/unit-test-01-primer.md">Author and run tests</a>.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    <span>
                      <a href="https://msdn.microsoft.com/library/dd286726.aspx">Use UI Automation To Test Your Code</a>
                    </span>
                </p>
            </td>
            <td>
                <p>Windows only</p>
            </td>
            <td>
                <p></p>
            </td>
        </tr>
        <tr>
            <td rowspan="4">
                <p>
                    <strong>Diagnose</strong>
                </p>
            </td>
            <td>
                <p><a href="https://msdn.microsoft.com/library/dd264939.aspx">Analyzing Managed Code Quality by Using Code Analysis</a></p>
            </td>
            <td>
                <p>No</p>
            </td>
            <td rowspan="3">
                <p>These tools all rely on .NET code and do not presently work with JavaScript.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p><a href="https://msdn.microsoft.com/library/hh205279.aspx">Finding Duplicate Code by using Code Clone Detection</a></p>
            </td>
            <td>
                <p>No</p>
            </td>
        </tr>
        <tr>
            <td>
                <p><a href="https://msdn.microsoft.com/library/bb385910.aspx">Measuring Complexity and Maintainability of Managed Code</a></p>
            </td>
            <td>
                <p>No</p>
            </td>
        </tr>
        <tr>
            <td>
                <p><a href="https://msdn.microsoft.com/library/z9z62c29.aspx">Using Profiling Tools</a> (includes CPU Usage,
                    Energy Consumption, GPU Usage, HTML UI Responsiveness, and JavaScript Memory, and Memory Usage)</p>
            </td>
            <td>
                <p>Windows only</p>
            </td>
            <td>
                <p>On Windows, Cordova apps run as native Windows apps so all tools are available. These tools are not available
                    when running the app on other platforms.</p>
            </td>
        </tr>
        <tr>
            <td rowspan="3">
                <p>
                    <strong><a href="https://msdn.microsoft.com/library/dn217874.aspx">Automate deployments with Release Management</a></strong>
                </p>
            </td>
            <td>
                <p>Manage release processes</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p></p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Deployment to servers for side-loading via scripts</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p></p>
            </td>
        </tr>
        <tr>
            <td>
                <p>Upload to app store</p>
            </td>
            <td>
                <p>Partial</p>
            </td>
            <td>
                <p>See <a href="./vs-taco-package-publish.md">Package your Cordova app</a>. Also note that extensions are available
                    that can automate this process for some app stores. See <a href="https://marketplace.visualstudio.com/VSTS">Extensions for Visual Studio Team Services</a>,
                    especially the <a href="https://marketplace.visualstudio.com/items?itemName=ms-vsclient.google-play">extension for Google Play</a>.</p>
            </td>
        </tr>
        <tr>
            <td>
                <p>
                    <strong><a href="https://www.hockeyapp.net/features/">Monitor with HockeyApp</a></strong>
                </p>
            </td>
            <td>
                <p>Crash analytics, telemetry, and beta distribution</p>
            </td>
            <td>
                <p>Yes</p>
            </td>
            <td>
                <p></p>
            </td>
        </tr>
    </tbody>
</table>
