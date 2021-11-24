--- 
description: "With the release of Visual Studio 2015, you now have a number of options for how you can integrate Cordova apps with your favorite CI server thanks to the fact that projects created in Visual Studio are standard Apache Cordova CLI projects."
title: "Get Started with Continous Integration (CI)"
author: "jmatthiesen"
ms.prod: "visual-studio-dev14"
ms.author: jomatthi
--- 

# Get started with continous integration (CI)
With the release of Visual Studio 2015, you now have a number of options for how you can integrate Cordova apps with your favorite team / continuous integration (CI) server thanks to the fact that projects created in Visual Studio are standard [Apache Cordova Command Line Interface](https://go.microsoft.com/fwlink/?LinkID=533773) (CLI) projects.

## Visual Studio Team Services and Team Foundation Services 2015 quick start
Visual Studio Team Services (formerly Visual Studio Online) and Team Foundation Services (TFS) 2015 can take advantage of a pre-built Cordova tasks that add additional features to streamline setup like managaging certificates for iOS! Simply install the **[Visual Studio Team Services Extension for Cordova](https://go.microsoft.com/fwlink/?LinkID=691188)** and add a Cordova Build task to your build definition. 


<table style="width: 100%; border-style: none;"><tr>
<td style="width: 140px; text-align: center;"><img src="https://raw.githubusercontent.com/Microsoft/vsts-cordova-tasks/master/docs/media/misc/cordova_logo_white_purple.png" alt="cordova logo"/></td>
<td><strong>Visual Studio Team Services Extension for Cordova</strong><br />
<i>Streamline CI setup for your Apache Cordova, PhoneGap, Ionic, or Cordova CLI compatible app using a set of useful pre-defined build steps.</i><br />
<a href="https://go.microsoft.com/fwlink/?LinkID=691188">Install now!</a>
</td>
</tr></table>


See the quick start on the [extension](https://go.microsoft.com/fwlink/?LinkID=691188) page along with the detailed [Build Apache Cordova apps with Visual Studio Team Services or Team Foundation Services 2015](https://go.microsoft.com/fwlink/?LinkID=691186) tutorial.

## Gulp build quick start
If you would prefer not to use the extension or are not using VS Team Services or TFS, you can get your project running in nearly any CI system quickly using the [taco-team-build node module](https://go.microsoft.com/fwlink/?LinkID=533736) and Gulp. Note that the method described here can be used with Jenkins, as an alternative for VS Team Services or TFS 2015, and others but will **not** work with TFS 2013. See [Tutorials on Specific CI Systems](#ci) for details.

To get going quickly, follow these steps:

1. **Install dependencies:** First, either install Visual Studio with the Tools for Apache Cordova option on your build server or simply install the pre-requisites for the platforms you are targeting separately. In brief:
    * Visual Studio itself is **only** required if you are building for **Windows or Windows Phone**.
    * Install [Node.js](https://go.microsoft.com/fwlink/?LinkID=396867) and you'll also need to install the [Git command line tools](https://go.microsoft.com/fwlink/?LinkID=396870) on Windows. Note that the default option for Git's install does not place the tools in the path. Select option to run the tools from the command prompt.
    * **Android** requires the [Java](https://go.microsoft.com/fwlink/?LinkID=396871), [Ant](https://go.microsoft.com/fwlink/?LinkID=396869) (Cordova < 5.0.0 only), and the [Android SDK](https://go.microsoft.com/fwlink/?LinkID=533747) with the correct API level installed (usually API 21 or 22). Add environment variables for ANDROID_HOME pointing to your Android SDK install, ANT_HOME to your Ant install, and JAVA_HOME to your Java install. Add %ANT_HOME%\bin to your path.
    * **iOS** requires Xcode (from the Mac app store) and [Node.js](https://go.microsoft.com/fwlink/?LinkID=396867)
    * See the [General CI tutorial](ci-guide.md#depends) or [Install Dependencies Manually](https://msdn.microsoft.com/library/dn771551.aspx) in MSDN for some information on what to install for a given platform.

2. **Update package.json in your project:** Add these lines to a package.json file in your project. Use [this version](https://go.microsoft.com/fwlink/?LinkID=691923) if you do not yet have a package.json file in the root of your project.

    ~~~~~~~~~~~~~~~~~~~~~~~~~~
    {
      "devDependencies": {
        "gulp": "^3.9.1",
        "gulp-typescript": "^2.11.0",
        "taco-team-build": "^0.2.2"
      }
    }
    ~~~~~~~~~~~~~~~~~~~~~~~~~~

3. **Add a gulpfile to your project:** Add [this Gulp file](https://go.microsoft.com/fwlink/?LinkID=691922) to the root of your project.

	![gulpfile.js in project](media/tutorial-team-build-readme/quick-1.png)

4. **Try it locally:** Test out your build by dropping to the command line, and running the following commands from the root of your Cordova project (not the solution root):

    ~~~~~~~~~~~~~~~~~~~~~~~~~~
    npm install
    node_modules\.bin\gulp
    ~~~~~~~~~~~~~~~~~~~~~~~~~~

    ...or on a Mac...

    ~~~~~~~~~~~~~~~~~~~~~~~~~~
    npm install
    ./node_modules/.bin/gulp
    ~~~~~~~~~~~~~~~~~~~~~~~~~~

    Gulp will now build Android and Windows versions of your project when run from Windows and iOS when run from OSX. You can change this behavior by updating the following lines in gulpfile.js.

    ~~~~~~~~~~~~~~~~~~~~~~~~~~
    var winPlatforms = ["android", "windows"],
        osxPlatforms = ["ios"],
        ...
    ~~~~~~~~~~~~~~~~~~~~~~~~~~

5. **Add to source control:** Assuming all goes well, add your project into the appropriate source code repository.

6. **Configure CI:** Next, configure your Team / CI server to fetch your project and execute the exact same commands mentioned above from the root of your Cordova project after fetching the source code. You can find detailed instructions for certain CI systems [below](#ci).

7. **Windows & OSX Build Agents:** Finally, configure an build agent on both Windows and OSX so you can build for any platform. See the tutorials below for some specifics on how to configure your CI system.

That's it!

## Tutorials on specific CI systems
<a name="ci"></a>
For additional detail on configuring specific build systems, see the following detailed tutorials:

*  **[Build Apache Cordova apps with Visual Studio Team Services or Team Foundation Services 2015](https://go.microsoft.com/fwlink/?LinkID=691186)**
*  **[Use the Visual Studio Tools for Apache Cordova with Team Foundation Services 2013](./tfs2013.md)**
*  **[Use the Visual Studio Tools for Apache Cordova with the Jenkins CI system](./jenkins.md)**

The following articles also provide some additional details and troubleshooting information:

*  **[Build a Cordova project by using Gulp](using-gulp-build-tasks.md)**
*  **[Comprehensive Guide to Continuous Integration with Cordova Apps](ci-guide.md)**

## More Information
* [Read tutorials and learn about tips, tricks, and known issues](../index.md)
* [Download samples from our Cordova Samples repository](https://github.com/Microsoft/cordova-samples)
* [Follow us on Twitter](https://twitter.com/VSCordovaTools)
* [Visit our site https://aka.ms/cordova](https://aka.ms/cordova)
* [Ask for help on StackOverflow](http://stackoverflow.com/questions/tagged/visual-studio-cordova)
