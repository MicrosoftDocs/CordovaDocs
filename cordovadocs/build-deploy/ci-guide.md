<properties
	pageTitle="Continuous Integration for Apache Cordova Apps"
  	description="Continuous Integration for Apache Cordova Apps"
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
	ms.date="02/11/2017"
	ms.author="johnwargo"/>

# Continuous Integration for Apache Cordova Apps

Visual Studio developers have a number of options for how you can integrate Cordova apps with your favorite continuous integration (CI) server thanks to the fact that projects created in Visual Studio are standard [Apache Cordova Command Line Interface](http://go.microsoft.com/fwlink/?LinkID=533773) (CLI) projects. Any build tools that work with Cordova application projects should work just fine with projects created with Visual Studio Tools for Apache Cordova (TACO). 

This article covers the general approach for tackling a number challenges that exist when building Cordova apps and cover what the [Visual Studio Team Services Extension for Cordova](http://go.microsoft.com/fwlink/?LinkID=691188) and [taco-team-build node module](http://go.microsoft.com/fwlink/?LinkID=533736) do behind the scenes to help you.

For additional information on specific build systems, you may find the Visual Studio Team Services (VSTS) [Cordova Build](http://go.microsoft.com/fwlink/?LinkID=691188) extension useful along with the following tutorials:

+	[Build Apache Cordova apps with Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkID=691186)
+	[Use the Visual Studio Tools for Apache Cordova with the Jenkins CI system](./jenkins.md)
+	[Build a Cordova project by using Gulp](./vs-taco-tutorial-gulp.md)

## <a name="whattoadd"></a> What to Add to Source Control

On the surface, it seems like all of the files in a Cordova project should be tracked through source control. However, many of the project's files and folders are build-time artifacts, created and used only during the build process, and not needed until it's time to build the project. All you really need are the following files and folders:

+	`config.xml`
+	`\hooks\`
+	`\merges\` (if you have one, the Cordova CLI doesn't make this folder automatically)
+	`\www\`

To ensure your project's platforms and plugins get added correctly during the build process, you'll want to save platform and plugin information to the project's `config.xml` file using the information provided in the Cordova documentation's [Platforms and Plugins Version Management](https://cordova.apache.org/docs/en/latest/platform_plugin_versioning_ref/). Essentially, you can store the platform and plugin information to the project's configuration by appending `--save` to the end of the `cordova plugin add` and `cordova platform add` commands. For example, to add the Android platform to a project and write the platform information to the project's `config.xml` file, execute the following command:

```
cordova platform add android --save
```

You can save information about all installed platforms at once using the following command:

```
cordova platform save
```  

The same is true for plugins, the following command adds the Cordova Device plugin to the project and saves the plugin information to the project's `config.xml` file:

```
cordova plugin add cordova-plugin-device --save
```

While the following command saves information about each installed plugin to the project's `config.xml` file:

```
cordova plugin save
```

When you execute these commands, the Cordova CLI adds entries to the `config.xml` for platforms and plugins (depending on which commands you execute). Platforms are added as `engine` elements and plugins are added as `plugin` elements as shown at the end of the following sample `config.xml`:  

```
<?xml version='1.0' encoding='utf-8'?>
    <widget id="io.cordova.hellocordova" version="1.0.0" xmlns="http://www.w3.org/ns/widgets" xmlns:cdv="http://cordova.apache.org/ns/1.0">
    <name>TACO Weather App</name>
    <description>
        A simple weather application
    </description>
    <author email="vscordovatools@microsoft.com" href="http://taco.visualstudio.com">Visual Studio JS Mobile Tooling</author>
    <content src="index.html" />
    <access origin="*" />
    <allow-intent href="http://*/*" />
    <allow-intent href="https://*/*" />
    <allow-intent href="tel:*" />
    <allow-intent href="sms:*" />
    <allow-intent href="mailto:*" />
    <allow-intent href="geo:*" />
    <platform name="android">
        <allow-intent href="market:*" />
    </platform>
    <platform name="ios">
        <allow-intent href="itms:*" />
        <allow-intent href="itms-apps:*" />
    </platform>
    <engine name="android" spec="~6.1.2" />
    <engine name="windows" spec="~4.4.3" />
    <plugin name="cordova-plugin-whitelist" spec="1" />
    <plugin name="cordova-plugin-console" spec="~1.0.5" />
    <plugin name="cordova-plugin-device" spec="~1.1.4" />
    <plugin name="cordova-plugin-notification" spec="~1.3.1" />
    <plugin name="cordova-plugin-geolocation" spec="~2.4.1" />
    </widget>
```

Later on, when a developer or your build process checks out the code, the Cordova `prepare` command (executed automatically as part of a Cordova app build) automatically adds the platforms and projects back to the project and you're ready to go. 

## <a name="depends"></a> Installing Dependencies

Cordova builds require that a number of dependencies be properly installed and configured on the system. However, exactly which dependencies are required varies based on the Cordova "platform" (Android, iOS, Windows) you want to build.

Installing Visual Studio 2017 with the Tools for Apache Cordova (TACO) option will automatically install these dependencies but you will still need to configure some of the environment variables by hand for Android. See [Build Apache Cordova apps](http://go.microsoft.com/fwlink/?LinkID=691186) for a summary of these variables. Otherwise you can manually install only those dependencies that are needed for building the platforms you are interested in.

1.	Install Node.js and make sure it is available to the system user you intend to have run your builds. The VSTS [Cordova Build extension](http://go.microsoft.com/fwlink/?LinkID=691188) and automatically ensures the correct version of Node.js is used for a given version of Cordova. Otherwise make sure you install the latest supported version of NodeJS for your target Cordova CLI version. Recent CLI releases are usually compatible with the latest long term support (LTS) version available at [https://nodejs.org](https://nodejs.org). 

2.	Install the platform specific dependencies on the server and make them available to the same user. See the following guides for details:

    1.	[Android Platform Guide](http://go.microsoft.com/fwlink/?LinkID=533774). A few notes:

        +	You do not need to install Android Studio. Instead you may download and install one of the ["SDK Tools Only" packages](http://go.microsoft.com/fwlink/?LinkID=533747). Click the **Download Options** link on the page to quickly get to the download.
        
		+	When building, you may encounter an error telling you that you need to install specific SDK versions or tools depending on the version of Cordova you are using. Note that these messages are talking about the *tools and SDK* versions *not* the device target versions.
        	+	You can install additional SDKs using [the Android SDK Manager](http://go.microsoft.com/fwlink/?LinkID=533775).
        	+	Note that only the "SDK Platform" is required for a given API level so you may uncheck other options. Android system images in particular are large and are not needed.
            +	Be sure to also install the **platform tools**            

    2.	macOS ony: [iOS Platform Guide](http://go.microsoft.com/fwlink/?LinkID=533776). You do not need to install the deployment tools mentioned.

    3.	Windows only: See the [Windows Platfrom Guide](http://go.microsoft.com/fwlink/?LinkID=533777).        

## <a name="proxy"></a> Internet Access & Proxy Setup

If your build server is running in a data center, it may be locked down and not have unrestricted access to the Internet. Modern mobile development frameworks like Cordova and Ionic pull build components from online repositories during the build process. Because of this, you'll need to allow build servers to access the following external domains:

- npm: `http://registry.npmjs.org`
- GitHub: `https://github.com`
- Apache ASF Git: `https://git-wip-us.apache.org`
- gradle.org: `http://services.gradle.org`
- maven.org: `https://*.maven.org`

If you need to use a proxy, you will need to configure both Node Package Manager (npm) and Git command line tools to use them. Open a command prompt on Windows or the Terminal app on macOS and execute the following commands:

```
npm config set proxy http://<username>:<password>@<host>
npm config set https-proxy http://<username>:<password>@<host>
git config --global http.proxy http://<username>:<password>@<host>
```

Where `<username>:<password>@` is optional and should contain the appropriate user name and password for proxy access while `<host>` is the correct proxy host and port (ex: `myproxy.mycompany.com:8080`).

You may also need to configure proxy settings for Java. This can be [accomplished via the Java control panel (recommended)](http://java.com/en/download/help/proxy_setup.xml) or by setting an environment variable as the follows:

```
JAVA_OPTS="-Dhttps.proxyHost=<host> -Dhttps.proxyPort=<port> -Dhttp.proxyHost=<host> -Dhttp.proxyPort=<port> -DproxySet=true"
```

Finally, if you see the error "**TypeError: Request path contains unescaped characters**" when building or installing a plugin you may need to either upgrade to a more recent version of Node.  See [tips and workarounds](../tips-and-workarounds/general/tips-and-workarounds-general-readme.md#cordovaproxy) for additional details.

## <a name="osxgotcha"></a> iOS/OSX Gotchas: Troubleshooting Tips for Building on a Mac

There are a few relatively common issues when building a Cordova app on macOS related to permissions that are worth noting.

1.	**You are seeing permission errors from "npm":** If you are seeing npm permission errors, you may be running into a situation where the build agent user's cache folder (`~/.npm`) is inaccessible. Generally this occurs if the folder or some of its contents were created while running as an administrator (perhaps using the `sudo` command). Fortunately this is easy to resolve:

    1.	Log into macOS as the user that installed and set up the cross-platform agent.

    2.	Open the Terminal app and type:

        ```
        sudo npm cache clear
        ```

    3.	Next, type:

        ```
        sudo chown -R `whoami` ~/.npm
        ```

2.	**You checked in the `hooks` folder from Windows and are seeing `spawn EACCES errors`**: If you encounter a `spawn EACCES` error when building on macOS or Linux, be sure all of the files in the Cordova project's `hooks` folder has their execute attribute set. Set the execute attribute on the files in source control or use the linux `chmod` command as a part of your build script; for example:

	```
	chmod +x file_name
	```

	This is commonly seen with the **Ionic framework** due to the hook in `hooks/after_prepare` step.
        
3.  **You checked in the `platforms` folder from Windows and are seeing permission errors:** 
 
	You should not run into this situation if you are using the VSTS [Cordova Build](http://go.microsoft.com/fwlink/?LinkID=691188) extension, but if you are seeing errors that are originating from files in your project's `platforms` folder, the root cause may be that you checked in shell scripts under the `platforms/android/cordova` or `platforms/ios/cordova` folders from Windows. This is because the NTFS file system has no concept of the **execute** permission required to run these from macOS. The `platforms` folder is not intended to be checked in and by default are excluded from Cordova projects in Visual Studio as a result.

    For example, this error is saying the "version" script is not executable:

	```
    [17:41:57] Error:
    /Users/vsoagent/vsoagent/agent/work/build/b424d56537be4854de825289f019285698609afddf826d5d1a185eb60b806e47/repo/tfs-vnext test/platforms/android/cordova/version:
    Command failed with exit code EACCES
   ```

   To resolve this problem you have two options:
   
   1.  Don't check in the contents of the Cordova project's `platforms` folder into source control. The folder and associated platform files will be added automatically at build time by the Cordova CLI anyway.	
   2.  If you absolutely must check in the contents of the platforms folder from Windows, you can craft a shell script to set the execute attributes on these files and include it as a part of your build process. There is a [Cordova hook based version of this script](../tips-and-workarounds/ios/osx-set-execute/tips-and-workarounds-ios-osx-set-execute-readme.md) available in the tips and workarounds section.

## <a name="basic"></a> Behind the Scenes: Basic Workflow

In general, we recommend following one of the tutorials above. Each build server technology is a bit different and in this article we will focus on the general steps required to build a Cordova app regardless of technology using the Cordova Command Line Interface (CLI).

The basic flow for building a Cordova app is simple on the surface:

1.  Check the project source code out from source control.

2.  If you saved the Cordova project's platform and plugin information to the project's `config.xml` file using the commands discussed earlier, the platforms and plugins will be added automatically during the build process. If you didn't, you'll need to add the platforms and plugins to the project manually using the `cordova platform add` and `cordova plugin add` CLI commands. 

3.  Build the project using the `cordova build` CLI command:

	```
   	cordova build android --release
	```

	or

	```
	cordova build ios --release
	```

The Cordova CLI is node.js based, so these exact same steps can be run from Windows or an macOS system or from a cloud hosted virtual machine (vm) like [MacInCloud](http://go.microsoft.com/fwlink/?LinkID=533746). See the [Cordova CLI documentation](http://go.microsoft.com/fwlink/?LinkID=533773) for additional details.

Exactly how these steps are executed will vary depending on your build server. However, there are a number of challenges that may not be immediately obvious when setting up an automated build environment. The remainder of this article will describe some techniques for dealing with these common problems.

### A Note on TypeScript

Unlike Visual Studio, it's important to note that the base Cordova CLI does not itself automatically compile TypeScript code. If you are using a build language like **Gulp** or **Grunt**, there are convenient plugins that you can use to compile your TypeScript code. Otherwise there is also a node.js based command line utility that works both on Windows and macOS. See the following links for additional details:

-   [Compiling TypeScript from the command line](http://go.microsoft.com/fwlink/?LinkID=533802)
-   [gulp-typescript](http://go.microsoft.com/fwlink/?LinkID=533748)
-   [grunt-typescript](http://go.microsoft.com/fwlink/?LinkID=533779)

## <a name="challenges"></a> Behind the Scenes: Resolving Common Cordova Challenges

When building Cordova projects in a server environment, there are a number of challenges you may encounter. If you are looking for a quick solution you should consider using the VSTS [Cordova Build](http://go.microsoft.com/fwlink/?LinkID=691188) extension or the [Gulp](./vs-taco-tutorial-gulp.md) tutorial as they are specifically designed to help resolve these problems regardless of build system.  If you're interested in understanding what is going on behind the scenes, consider the following:

1.	**Building with Multiple Versions of the Cordova CLI.** While in an ideal world everyone would use the edge version of the Cordova CLI and associated platforms, the reality is that for any build server you will need to support multiple versions of the Cordova CLI. This means that the common practice of installing Cordova globally will not work; each build would use its own Cordova CLI and NodeJS installations.

2.	**Generating an iOS App Store Package.** A common issue that can arise is keychain permission errors during app signing that result in strange "User interaction not allowed" errors. 

### <a name="multicli"></a> Building with Multiple Versions of the Cordova CLI

The Cordova CLI is a standard Node.js npm package and thus can be installed either globally or locally. For build environments, the trick is to use a local installation of the Cordova CLI rather than a global one. There are two different methods that you can use to install Cordova locally: at the project level or in a global cache.

#### Project Level

Installing and using the correct version of the Cordova CLI at the project level is simple thanks to a project's [`package.json`](http://go.microsoft.com/fwlink/?LinkID=533781) file. Here is the general approach:

1.	Create a `package.json` file in the root of your Cordova project.

2.	Add the following JSON to the file where `6.0.0` is the version of the Cordova CLI you intend to use:

	```
    {
    	"devDependencies": {
    		"cordova": "6.0.0"
	    }
	}
	```

3.	Check this file into source control with your project.

4.	Configure your build system to run the following command as its first task:

	```
	npm install
	```

	This will install the specified (in the project's `package.json` file) version of the CLI in the `node\_modules` folder local to your project.

5.	When executing any Cordova CLI commands for your build task, use the following form of the commands:

	Windows:

	```
	node_modules\cordova\bin\cordova
	```

	OSX:

	```
	./node_modules/cordova/bin/cordova
	```

    Ex:

	```
	./node_modules/cordova/bin/cordova platform add android
	```

	The downside of this method is that you will end up installing the Cordova CLI each time you execute a clean build which will slow down your build times particularly on Windows as the CLI consists of around 25MB of small files.

#### Global Cache

To avoid re-installing the Cordova CLI every time, you can take advantage of Visual Studio's **taco.json** file and a Node.js script to perform the installation in a specific location that you then use to execute Cordova commands.

1.	Add an environment variable to your system (or build) called `CORDOVA_CACHE` pointing to where you want to store the cache of different Cordova CLI versions used to build your projects.

2.	Add the following JavaScript file to the project and call it `setup-cordova.js`:

	```JavaScript
    var fs = require("fs"),
        path = require("path"),
        exec = require("child_process").exec;

	//Update this with the correct Cordova version
    var cordovaVersion = "6.0.0";

    // Check if Cordova is already present in the cache, install it if not
    var cordovaModulePath = path.resolve(process.env["CORDOVA_CACHE"], cordovaVersion);
    if (!fs.existsSync(cordovaModulePath)) {
        fs.mkdirSync(cordovaModulePath);
        fs.mkdirSync(path.join(cordovaModulePath, "node_modules"));
        console.log("Installing Cordova " + cordovaVersion + ".");
    	exec("npm install cordova@" + cordovaVersion, { cwd: cordovaModulePath }, function (err, stdout, stderr) {
            console.log(stdout);
            if (stderr) {
                console.error(stderr);
            }
            if (err) {
                console.error(err);
                process.exit(1);
            }
    		console.log("Cordova " + cordovaVersion + " installed at " + cordovaModulePath);
        });
    } else {
    	console.log("Cordova " + cordovaVersion + " already installed at " + cordovaModulePath);
    }

    // Create shell scripts
    if (process.platform == "darwin") {
        // OSX
	    fs.writeFileSync("cordova.sh", "#!/bin/sh\n" + path.join(cordovaModulePath, "node_modules", "cordova", "bin", "cordova") + " $@", "utf8");
        fs.chmodSync("cordova.sh", "0777")
    } else {
        // Windows
    	fs.writeFileSync("cordova.cmd", "@" + path.join(cordovaModulePath, "node_modules", "cordova", "bin", "cordova") + " %*", "utf8");
    }
	```

3.	In your team / CI build definition or script, add a build task to execute `node setup-cordova.js` from your project root

4.	Use `./cordova.sh` (macOS) or `cordova.cmd` (Windows) to run additional Cordova commands as needed.

	A variation of this method is used by Visual Studio TACO, the VSTS [Cordova Build](http://go.microsoft.com/fwlink/?LinkID=691188) extension, and the taco-team-build Node module referenced in the [Gulp](./vs-taco-tutorial-gulp.md) and [Jenkins](./jenkins.md) tutorials.

### <a name="platforms"></a> Adding Platforms

Adding platforms in Cordova is quite simple using the `cordova platform add` command. Ex:

```
cordova platform add android
```

However, there are a couple of common problems when executing this command that you could run into.

1.	**Platform Download Messages Result in Build Failures.** Where things can get a bit tricky is that Node.js emits warnings to `stdout`.  The issue is that the Cordova CLI `cordova platform add` command can result in warnings being reported when the CLI is downloading a version of a given Cordova platform for the first time. This is not an error, but some build systems will assume anything sent to `stderr` means a build failure occurred.

    Many CI systems provide a "continue on error" option that you can select to get around this particular problem or you can pipe `stderr` to `stdout` if you'd prefer as shown in the following example:
    
	```
	cordova platform add ios 2>&1
	```

2.	**Errors During Incremental Builds.** If you are doing an incremental build and the platform you are building has already been added, the resulting exit code will be non-zero and may be interpreted as a build failure. If your build system supports a "continue on error" option for a given task, you can simply select that.

	However, a more robust solution is to simply conditionally call `platform add` if the appropriate folder in the platforms folder in your project is not found. In the scripts below replace `cordova` with the appropriate command from the "Building with Multiple Versions of the Cordova CLI" section above.

    Windows:

    ```
    IF NOT EXIST platforms/android CALL cordova platform add android
    ```

    OSX:

    ```
    if [ ! -d "platforms/android" ]; then cordova platform add android; fi;
    ```

### <a name="ipa"></a> Generating an iOS App Store Package 

#### Solving iOS Keychain Permission Errors

When building for iOS you may run into permissions issues when using a build server like [Jenkins](http://go.microsoft.com/fwlink/?LinkID=533784) because the build agent does not have permissions to access the login keychain were your installed signing certificates are located. In this case, the codesign utility then attempts to pop-up a dialog where you enter the appropriate password but typically this won't be possible since you are not logged in interactively. You will then see a very cryptic **User interaction not allowed** error.

The VSTS [Cordova Build](http://go.microsoft.com/fwlink/?LinkID=691188) extension has some useful features designed to avoid this problem entirely such as supporting referencing a P12 certificate file directly in your build definition. In other cases you'll need to unlock the login keychain before you build and package your Cordova app.

Most build servers provide a way to inject secure environment variables before executing build tasks. In Jenkins this is accomplished by using the **Environment Injector Plugin**. By then setting a `KEYCHAIN_PWD` environment variable you can add the following command to your build:

```
security unlock-keychain -p ${KEYCHAIN_PWD} ${HOME}/Library/Keychains/login.keychain
```

To then build and package your iOS app, you can run the following commands:

```
security unlock-keychain -p ${KEYCHAIN_PWD} ${HOME}/Library/Keychains/login.keychain
cordova build ios --device --release
xcrun -v -sdk iphoneos PackageApplication "${WORKSPACE}/platforms/ios/build/device/My Cordova App.app" -o "${WORKSPACE}/platforms/ios/build/device/My Cordova App.ipa"
```

#### Generating an "ipa" on Old Versions of Cordova

In order to distribute your iOS application you will need to generate an **iOS App Store Package** (an `.ipa` file). These files can be imported into iTunes or enterprise app stores in addition to being distributed to the Apple App Store via the [Application Loader](http://go.microsoft.com/fwlink/?LinkID=533751). Recent versions of the Cordova CLI support a `build.json` file that is used to specify signing information and will automatically generate an `.ipa` file for you. Older versions of Cordova do not support this feature a Xcode command line tool needs to be used instead. Ex:

```
xcrun -v -sdk iphoneos PackageApplication source.app -o dest.ipa
```

In Cordova projects, the source `.app` package can be found in the `platforms/ios/build/device` folder in your project after a successful Cordova device build. As an important detail, `source.app` and `dest.ipa` above should be **absolute paths** and the name of the package is taken from the **Display Name** (`widget/@name`) in the project's `config.xml` file, which may not match your project folder name.

```
xcrun -v -sdk iphoneos PackageApplication "/Users/cdvusr/Documents/cordova/myapp/platforms/ios/build/device/My Cordova App.app" -o "/Users/cordova/Documents/cordova/myapp/platforms/ios/build/device/My Cordova App.ipa"
```

Each build system has a different mechanisms in place for passing the absolute path of the project to shell scripts, but typically it involves the use of an environment variable. For example, in Jenkins you can use the following:

```
xcrun -v -sdk iphoneos PackageApplication "${WORKSPACE}/platforms/ios/build/device/My Cordova App.app" -o "${WORKSPACE}/platforms/ios/build/device/My Cordova App.ipa"
```

## More Information
* [Learn about other Team Build / CI options](./tutorial-team-build-readme.md)
* [Read tutorials and learn about tips, tricks, and known issues](../cordova-docs-readme.md)
* [Download samples from our Cordova Samples repository](http://github.com/Microsoft/cordova-samples)
* [Follow us on Twitter](https://twitter.com/VSCordovaTools)
* [Visit our site http://aka.ms/cordova](http://aka.ms/cordova)
* [Ask for help on StackOverflow](http://stackoverflow.com/questions/tagged/visual-studio-cordova)
