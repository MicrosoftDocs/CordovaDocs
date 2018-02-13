<properties pageTitle="Using mocks"
  description="Using mocks"
  services=""
  documentationCenter=""
  authors="Kraig Brockschmidt" />

#Using mocks for platform APIs, plugins, and other external dependencies

Unit testing executes code in a runtime that’s separate from the runtime in which the app will eventually run on a mobile platform. Unit testing thus happens without deploying the code to an emulator or device that’s running the platform operating system.

> **Note:** It is certainly possible, of course, to include your unit test code with a build of the app that’s deployed to a mobile platform, and to execute the tests on that platform directly. This is a good test to run at some point, in fact, but not as part of a build or continuous integration process.

As a result, platform APIs are not available during unit testing, and any calls made to them—such as those that happen within a Cordova plugin—will fail. The same is also true of other external dependencies that the unit testing runtime doesn’t have access to, such as external databases and web services, and even the ability to authenticate with those resources.

At the same time, the purpose of unit testing is to validate only the correct operation of the app code; it need not validate an app’s interactions with a platform or external dependencies (this is a process called integration testing). Therefore unit tests don’t need to test an app’s communication with its backend or web services, and don’t need to test whether an operating system feature like geolocation actually works. But unit tests do need to validate how the app processes data that will be returned from such platforms and services.

The example we’ve been using in this tutorial, ```normalizeData```, isn’t concerned with where the data will ultimately come from. The unit tests, then just provide that data directly, as would be true of any other isolated processing functions like ```normalizeData```.

Other code units in an app, however, will call functions like ```normalizeData``` as part of a sequence of business logic. Consider the following function (in pseudo-code, ignoring error handling) that gathers data from two services, normalizes that data, and then combines the records to store in a database:

```javascript
function consolidateUserRecord(userID) {
    var jsonP = getPersonData(userID);          // Makes a web request
    var normP = normalizePersonData(jsonP);

    // [Check for existence of required fields and return null if any are missing]

    var jsonE = getEmploymentData(userID);      // Makes a web request
    var normE = normalizeEmploymentData(jsonE);

    // [Check for existence of required fields and return null if any are missing]

    var employeeRecord = {
        id: userID,
        employeeID: normE.employeeID,
        firstName: normP.FirstName,
        lastName: normP.LastName,
        email: normE.alias + "@contoso.com",
        title: normE.title,
        workPhone: normE.phone,
        homePhone: normP.phone        
    }

    var timestamp = dateTimePlugin.now();                   // Calls a platform API
    App.database.insertOrUpdate(employeeRecord, timestamp); // Depends on the database
    return employeeRecord.employeeID;
}
```

When unit testing this function, we first assume that ```getPersonData```, ```getEmploymentData```, ```normalizePersonData```, and ```normalizeEmploymentData``` are all unit tested separately. We assume the same about library functions like ```database.insertOrUpdate``` and components like the ```dateTimePlugin```. A unit test for ```consolidateUserRecord```, in short, focuses on the behavior of this function by itself, and not on the operation of its building blocks.

Still, when executing ```consolidateUserRecord``` in a unit testing runtime, we have to expect the following:
- The platform API used from the ```dateTimePlugin``` isn’t available.
- The production database isn’t available.
- External web requests will fail.

> **Note: **we don’t in fact want to make external web requests from a unit test in the first place because ultimately we want to run unit tests as part of a continuous integration build. Unit tests should run quickly and not depend on connectivity, nor should we break the build because of a network timeout. Again, these are matters for integration testing.

How, then, can we run this code at all? The answer is simple: we fake it, which means creating stubs—or mocks, as they’re commonly called—that intercept calls to external dependencies to mimic the data they return.

It won’t take long for you to realize that mocks must isolate and intercept all platform calls and calls to external dependencies.
 
There are several ways this can happen:

1.	A Cordova plugin might provide for mocks directly, because the plugin after all is entirely aware of any platform APIs it using. This is especially helpful with plugins that use third party external dependencies like databases as opposed to standard platform APIs.
2.	Some JavaScript libraries may already have mocking built in, or there exists a compatible framework for that purpose. For example, mockjax intercepts jQuery ```$.getJSON``` calls to return test data instead of hitting a real service. (For an example, see [http://www.telerik.com/blogs/using-qunit-to-unit-test-phonegap-cordova-applications](http://www.telerik.com/blogs/using-qunit-to-unit-test-phonegap-cordova-applications).)
3.	The [Cordova Mocks extension for Chrome](https://chrome.google.com/webstore/detail/cordova-mocks/iigcccneenmnplhhfhaeahiofeeeifpn) injects mock data for nine of the most common Cordova plugins when using Chrome as the unit testing runtime. The extension, in other words, provides mocking at the runtime level.
4.	Use plugins through a wrapping layer that provides for mocking intercepts. [ngCordova](http://ngcordova.com/) is an example of this, adding more than 70 AngularJS extensions on top of the Cordova API. This has the advantage over #3 above of being runtime-independent. 

To return now to our example of ```consolidateUserRecord```, here’s how we’d address the expected failures during unit testing:

- Instead of using the ```dateTimePlugin``` directly, access it through a wrapper than can intercept calls and provide mock data. The code impact is minimal because it requires us to only change the one line where ```dateTimePlugin``` is instantiated; the rest of the code can just use that same variable. If the plugin we’re using lacks mocks itself or isn’t covered by a tool like ngCordova, then it’s a small amount of code to wrap at least those parts of the plugin that you’re using.
- Using a production database will likely happen through a plugin, or through some other layer you’ve created to work with the database API. Either way, the wrapping approach works the same.
- With external web requests, if you’re using call like jQuery’s ```$.getJSON```, then you can use something like mockjax; if you’re using platform API calls instead, then it’s no different from mocking any other plugin.

It’s important to understand that for the purposes of unit testing, it’s not necessary to wrap and mock every possibly API call that your app might make. That is, you’re not trying to build a general-purpose API wrapping layer: you need only worry about those APIs you’re actually using. As a precaution, though, it’s a good idea to stub out the other methods in a plugin’s API to throw an error in case you write code to use that API later on. The error will remind you to improve the wrapping layer.

Also, it helps to keep in mind the relationships between the different parts your wrapping. In the ```consolidateUserRecord``` example, we’d be mocking calls to the ```database.insertOrUpdate``` method. Because we’re not using its return value, that method could—for at least this one example—just be a no-op. However, we’ll certainly be using a method like ```database.query``` elsewhere in the code, which would except to get back some useful information.

Question is, does ```database.insertOrUpdate``` actually need to store real data in some temporary location? The answer is no, because the data that we give to that method in the first place comes from other web request methods that are also being wrapped and mocked. We know there’s a relationship there, and so we can just shortcut the process and mock the data returned from ```database.query``` to match what ```insertOrUpdate``` might have been given.
