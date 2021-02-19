--- 
title: "Evaluate the performance costs of a Cordova app"
author: "jmatthiesen"
ms.prod: "visual-studio-dev14"
ms.topic: "troubleshooting"
ms.date: "10/08/2015"
ms.author: jomatthi
--- 

# Evaluate the performance costs of a Cordova app

Understanding the performance costs of Cordova will help you as you evaluate the platform, and plan ahead as you develop for it. There are some areas that deserve special attention: startup and resume overhead due to the interpreted nature of the web languages, memory overhead from hosting an entire browser in your app, serialization overhead when sending data to and from native code, and the general performance concerns that come with web applications.

> **Note**: We used a Nexus 7, iPad Mini 3, and Lumia 928 to produce most of the results that you'll see presented in this topic.

## <a id="startup"></a>The startup cost

The first impression you get to make is when a user launches your app for the first time. Are they pulled right into the app or is there a long loading period? Launching a Cordova app feels almost like setting off a Rube Goldberg machine: you’re launching a native app that hosts a WebView that points to an HTML file that initializes your application and pulls in all your JavaScript and HTML templates. Unsurprisingly, this can take a moment.

We don’t know the complexity of your app, but we experimented with launch times for a very basic "Hello World" app that updated some text on page when the “deviceready” event fires. For the timings in the table below, we used a camera to measure the time from when the touch was visually registered until we could see the text update. Additionally, we define “cold” as launching the app immediately after a reboot (when assets should be completely cleared from RAM) and “warm” as launching the app right after we close it (when maybe the operating system hasn’t really cleaned everything up yet).

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
<tbody><tr>
  <th>
    <p><strong>Platform</strong></p>
  </th>
  <th>
    <p><strong>Cordova Cold</strong></p>
  </th>
  <th>
    <p><strong>Native Cold</strong></p>
  </th>
  <th>
    <p><strong>Cordova Warm</strong></p>
  </th>
  <th>
    <p><strong>Native Warm</strong></p>
  </th>
</tr>
 <tr>
   <td><strong>Android 4.4</strong></td><td>3425 ms</td><td>557 ms</td>
   <td>3358 ms</td><td>454 ms</td>
 </tr>
 <tr>
   <td><strong>iOS 8</strong></td><td>3142 ms</td><td>5825 ms</td>
   <td>1921 ms</td><td>2000 ms</td>
 </tr>
 <tr>
   <td><strong>WP 8.0</strong></td><td>2433 ms</td><td>1667 ms</td>
   <td>1083 ms</td><td>1098 ms</td>
 </tr>
</table>

If you’re experiencing large delays when your app starts up, see what you tasks you can defer or cut to minimize your time. Are you requesting all your resources when the app launches or can some of those resources be deferred to load on demand? Does the UI library that you're using include components that your app is not using?

## <a id="resume"></a>The resume cost

Just like when your app starts, resuming your app doesn't happen instantly either. If your app needs to do anything to respond to the “resume” event, you’ll want to keep an eye on this.

We tested this with a “Hello World” app that changed its background color when the “resume” event fired. Then, by using a camera, we measured the time from tapping on the app and the switching of the screen being visually registered, until the background color was updated. We don’t have comparisons for native apps, but it seemed like the times that transpired were based more on the animation that the operating system used to smoothly bring the app to full screen rather than to wake the app up.

<table>
<tbody><tr>
  <th>
    <p><strong>Platform</strong></p>
  </th>
  <th>
    <p><strong>Time</strong></p>
  </th>
  </tr>
 <tr>
   <td><strong>Android 4.4</strong></td><td>450 ms</td>
 </tr>
 <tr>
   <td><strong>iOS 8</strong></td><td>58.3 ms</td>
 </tr>
 <tr>
   <td><strong>WP 8.0</strong></td><td>595.8 ms</td>
 </tr>
</table>

If you're experiencing these issues, see what code is running when you resume your app and if you can move that activity to another place.

## <a id="memory"></a>The memory cost

Cordova apps have a higher memory footprint than their native counterparts. Hosting what is essentially a browser inside of your app is not inexpensive. This can be a significant issue on low-end phones.

Android and iOS don't have hard numbers for allowable memory consumption. Each device and platform provides different usage allowances, and those allowances change based on how much memory the device has, and how much is in use at any given time. Windows Phone has some hard limits, ranging from 150MB to 825MB.

For a benchmark, here is the memory usage we recorded for "Hello World" apps across the three platforms. For the browser, we navigated to *about:blank*.

<table>
<tbody><tr>
  <th>
    <p><strong>Platform</strong></p>
  </th>
  <th>
    <p><strong>Native</strong></p>
  </th>
  <th>
    <p><strong>Browser</strong></p>
  </th>
  <th>
    <p><strong>Cordova</strong></p>
  </th>
</tr>
 <tr>
   <td><strong>Android</strong></td><td>26 MB</td><td>14 MB</td>
   <td>59 MB</td>
 </tr>
 <tr>
   <td><strong>iOS</strong></td><td>7 MB</td><td>20 MB</td>
   <td>18 MB</td>
 </tr>
 <tr>
   <td><strong>WP</strong></td><td>30 MB</td><td>23 MB</td>
   <td>36 MB</td>
 </tr>
</table>

While we haven't compared a full-featured app implemented both natively, and with Cordova across all three platforms, we did compare a native app across the platforms. This is a data point worth keeping in mind.

In the table below, you'll see that Android consumed more memory and iOS consumed less. To obtain these numbers, we launched the Facebook app, loaded the news feed, a profile page, and then recorded the memory usage.

<table>
<tbody><tr>
  <th>
    <p><strong>Platform</strong></p>
  </th>
  <th>
    <p><strong>Memory</strong></p>
  </th>
</tr>
 <tr>
   <td><strong>Android 4.4</strong></td><td>101.4 MB</td>
 </tr>
 <tr>
   <td><strong>iOS 8</strong></td><td>75.10 MB</td>
 </tr>
 <tr>
   <td><strong>WP 8.1</strong></td><td>90.42 MB</td>
 </tr>
</table>

If you’re struggling with these issues, see if your app is allocating large DOM trees or keeping extra copies of data in memory. Try virtualizing things that normally require large DOM trees, such as lists or tables, and make sure you are letting go of data you don’t need so that the garbage collector can dispose of it.

## <a id="communication"></a>The communication cost

Due to the architecture of Cordova which provides a thin native shim hosting a WebView, moving application state from JavaScript to the native side and back requires expensive serialization and deserialization. You might be reading and writing files, getting photos from the camera, or getting the results of computationally expensive calculations performed in native code. The good news is that our testing indicated this time is linearly related to the size of the data being transferred. The table below shows the times that we recorded when sending an array of numbers across the barrier and back.

<table>
<tbody><tr>
  <th>
    <p><strong>Items</strong></p>
  </th>
  <th>
    <p><strong>1</strong></p>
  </th>
  <th>
    <p><strong>2,000</strong></p>
  </th>
  <th>
    <p><strong>20,000</strong></p>
  </th>
  <th>
    <p><strong>200,000</strong></p>
  </th>
</tr>
 <tr>
   <td><strong>Android 4.4</strong></td><td>4 ms</td><td>22 ms</td>
   <td>157 ms</td><td>1121 ms</td>
 </tr>
 <tr>
   <td><strong>iOS 8.0</strong></td><td>3 ms</td><td>45 ms</td>
   <td>135 ms</td><td>1120 ms</td>
 </tr>
 <tr>
   <td><strong>WP 8.1</strong></td><td>1 ms</td><td>27 ms</td>
   <td>139 ms</td><td>877 ms</td>
 </tr>
</table>

If you’re struggling with this, make sure you that you know what is being sent back and forth and try to figure out how to reduce it. Look for duplicate data or maybe even experiment with different data formats to see if one serializes faster than others.

## <a id="web"></a>The web cost

Although last here, perhaps the most important cost to consider, is the cost that comes from building with web technologies. This is a cost that your team may have already paid if you have experience building applications for the modern web. If you’re new to frontend web technologies, the learning curve appears deceptively low. This is not a magic bullet: just like any other tech stack, building a performant application requires planning and knowledge of the pitfalls. If you’re struggling with this, read this article: [Optimize the performance of a Cordova app](better-web-performance.md).

## <a id="answer"></a>Final thoughts

One of the key attractors to Cordova is that you use web technology to build cross-platform apps. If your team has a strong web background or has an existing web app, leveraging that experience and those assets while tracking the costs listed here should enable you to bring a quality app to your customers.
