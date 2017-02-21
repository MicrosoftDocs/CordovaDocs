<properties 
	pageTitle="Get Started with Continuous Integration (CI)"
  	description="Get Started with Continuous Integration (CI)"
  	services=""
  	documentationCenter=""
  	authors="clantz, johnwargo" />
<tags 
	ms.technology="cordova" 
	ms.prod="visual-studio-dev15" 
	ms.service="na" 
	ms.devlang="javascript" 
	ms.topic="article" 
	ms.tgt_pltfrm="mobile-multiple" 
	ms.workload="na" 
	ms.date="02/10/2017" 
	ms.author="johnwargo"/>

# Get Started with Continuous Integration (CI)

Visual Studio developers have a number of options for how you can integrate Cordova apps with your favorite continuous integration (CI) server thanks to the fact that projects created in Visual Studio are standard [Apache Cordova Command Line Interface](http://go.microsoft.com/fwlink/?LinkID=533773) (CLI) projects. Any build tools that work with Cordova application projects should work just fine with projects created with Visual Studio Tools for Apache Cordova (TACO).

## Visual Studio Team Services

Development organizations using Visual Studio Team Services (VSTS) can add Cordova tasks to the build process using the **Cordova Build** cross-platform agent for VSTS. This new agent enables you to use VSTS build projects targeting Android, iOS, or Windows applications created using Tools for Apache Cordova or *any* Cordova compliant CLI like **Ionic** and **Adobe PhoneGap**.
 
<table style="width: 100%; border-style: none;"><tr>
<td style="width: 140px; text-align: center;"><img src="https://raw.githubusercontent.com/Microsoft/vsts-cordova-tasks/master/docs/media/misc/cordova_logo_white_purple.png" /></td>
<td><strong><a href="http://go.microsoft.com/fwlink/?LinkID=691188">Cordova Build</a></strong><br />
<a href="https://marketplace.visualstudio.com/search?term=publisher%3A%22Visual%20Studio%20Client%20Tools%22&target=VSTS">Visual Studio Client Tools</a><br />
<i>Streamline CI setup for your Apache Cordova, PhoneGap, Ionic, or Cordova CLI compatible app using a set of useful pre-defined build steps.</i><br />
<a href="http://go.microsoft.com/fwlink/?LinkID=691188">Install now!</a>
</td>
</tr></table>

See the quick start on the [extension](http://go.microsoft.com/fwlink/?LinkID=691188) page along with the detailed [Build Apache Cordova apps with Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkID=691186) tutorial.

## Gulp

If you would prefer not to use the extension or are not using VSTS, you can get your project running in nearly any CI system quickly using the [taco-team-build node module](http://go.microsoft.com/fwlink/?LinkID=533736) and Gulp. Note that the method described here can be used with Jenkins, as an alternative for VSTS, and others; see [Tutorials on Specific CI Systems](#ci) for details.

To use Gulp with your Cordova projects, follow these steps:

1.	**Install dependencies:** Install Visual Studio 2017 and the Tools for Apache Cordova on your build server or simply install the prerequisites for the platforms you are targeting separately:

    +	Visual Studio itself is **only** required if you are building for **Windows or Windows Phone**.
    +	Install [Node.js](http://nodejs.org) and you'll also need to install the [Git command line tools](https://git-scm.com/) on Windows. 
	    > **Note**: The default option for the Git installation does not add Git to the system `PATH` environment variable. To fix this, select the option to run the tools from the command prompt, this will add the Git installation folder to the system `PATH`.
    +	**Android** requires the [Java SDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) and the [Android SDK](http://go.microsoft.com/fwlink/?LinkID=533747) with the correct API level installed. Add environment variables for `ANDROID_HOME` pointing to your Android SDK installation folder, and `JAVA_HOME` to your Java install.
    +	**iOS** requires Xcode (from the Mac app store) and [Node.js](http://nodejs.org)
    +	See the [General CI tutorial](./vs-taco-ci-cordova.md#depends) or [Install Dependencies Manually](https://msdn.microsoft.com/en-us/library/dn771551.aspx) in MSDN for some information on what to install for a given platform.

2.	**Add or Update the `package.json` in your Cordova project:** Add the following development dependencies to your Cordova project's `package.json` file.

	> **Note**: You can use [this version](http://go.microsoft.com/fwlink/?LinkID=691923) if you do not yet have a `package.json` file in the root of your project.

	```JavaScript    
    {
      "devDependencies": {
        "gulp": "^3.9.1",
        "gulp-typescript": "^2.11.0",
        "taco-team-build": "^0.2.2"
      }
    }
    ```

3.	**Add a `gulpfile.js` to your project:** Add [this Gulp file](http://go.microsoft.com/fwlink/?LinkID=691922) to the root of your project.

	![gulpfile.js in project](media/va-taco-ci/quick-1.png)

4.	**Try it locally:** Test out your build by opening a command prompt (or terminal window on Macintosh), navigating to the Cordova project's root folder (the folder with the project's `www` folder), and executing the following commands:

    ```
    npm install
    node_modules\.bin\gulp
    ```

    ...or on a Mac...

    ```
    npm install
    ./node_modules/.bin/gulp
    ```

    Gulp will build Android and Windows versions of your project when run from Windows and iOS when run from OSX. You can change this behavior by updating the following lines in gulpfile.js.

    ```
    var winPlatforms = ["android", "windows"],
        osxPlatforms = ["ios"],
        ...
    ```

5.	**Add to source control:** Assuming all goes well, add your project into the appropriate source code repository.

6.	**Configure CI:** Configure your Team/CI server to fetch your Cordova project and execute the same commands mentioned above from the root of your Cordova project. You can find detailed instructions for certain CI systems [below](#ci).

7.	**Windows & OSX Build Agents:** Finally, configure an build agent / slave on both Windows and OSX so you can build for any platform. See the tutorials below for some specifics on how to configure your CI system.

That's it!

## <a name="ci"></a> Tutorials on specific CI systems

For additional information on how to configure specific build systems, see the following tutorials:

+	**[Build Apache Cordova Apps](http://go.microsoft.com/fwlink/?LinkID=691186)**
+	**[Use the Visual Studio Tools for Apache Cordova with Team Foundation Services 2013](./vs-taco-tfs2013.md)**
+	**[Use the Visual Studio Tools for Apache Cordova with the Jenkins CI system](./vs-taco-jenkins.md)**

The following articles provide additional details and troubleshooting information:

+	**[Build a Cordova project by using Gulp](../tutorial-gulp/vs-taco-tutorial-gulp.md)**
+	**[Comprehensive Guide to Continuous Integration with Cordova Apps](./vs-taco-ci-cordova.md)**

## Additional Information

+	[Download samples from our Cordova Samples repository](http://github.com/Microsoft/cordova-samples)
+	[Follow us on Twitter](https://twitter.com/VSCordovaTools)
+	[Visit our site http://aka.ms/cordova](http://aka.ms/cordova)
+	[Ask for help on StackOverflow](http://stackoverflow.com/questions/tagged/visual-studio-cordova)
