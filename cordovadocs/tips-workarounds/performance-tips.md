---
title: "Cordova performance tips"
description: "Cordova performance tips"
author: "kirupa"
ms.technology: "cordova"
ms.prod: "visual-studio-dev15"
ms.service: "na"
ms.devlang: "javascript"
ms.topic: "article"
ms.tgt_pltfrm: "mobile-multiple"
ms.workload: "na"
ms.date: "09/10/2015"
ms.author: "kirupac"
---

# Cordova performance tips

Cordova apps are web apps that run "inside" of native apps by using native "web view" UI components. There are therefore three major domains where performance can be affected in a Cordova app:

- The web code, in JS, CSS, and HTML
- The native code, in Objective-C/Swift, Java, or WinJS (Cordova plugin code).
- The bridge between the native and web code (Cordova runtime code).

## Optimizing web code, in JS, CSS, and HTML

There are *very many* things that can be optimized in a web app. The Internet contains a wealth of information on the topic. The optimizations that are highly relevant to Cordova apps appear below, but afterward we have a list of links for more in-depth reading.

### Removing the 300ms Touch Delay

There is a 300ms delay on some mobile browser platforms between a touch on the screen and its corresponding touch event being raised. For more responsive UI, this delay can be sidestepped. The `fastclick` library (available on [GitHub][fastclick]) conveniently implements this optimization.

### Layout Updates: Fewer is Better

HTML rendering engines recalculate a document layout when CSS rules or DOM element dimensions are changed. These recalculations are expensive, and minimizing them improves performance. Editing HTML elements outside of the DOM (e.g. creating new `<div>`s using jQuery) does not cause layout recalculations. Therefore editing HTML in JavaScript and then inserting the edited HTML into the DOM results in fewer layout recalculations and leads to improved performance.

### Images > CSS Gradients

CSS gradients take longer to render than static images on most HTML rendering engines. Using image gradient backgrounds in place of CSS gradients reduces frame render time.

### Transitions > Setting Properties

CSS3 transitions allow animation and transformation of HTML elements. They are implemented and optimized in most browsers, and usually perform better than "manual" animations (such as setting object `x` and `y` properties at defined intervals).

The Mozilla Developer Network guide on CSS transitions can be found [here][mdn_transitions].

### CSS Animation Loop > Custom Animation Loop

Browser JavaScript APIs expose a function called `requestAnimationFrame`. This allows a callback to be attached to the *rendering engine's frame-rendering loop*, which is already optimized and timed appropriately for rendering. Using it (instead of a custom event loop) to perform any per-frame changes performs better because the browser's internal rendering code is already heavily optimized.

## DOM

Nearly everything your app will do interacts with in the DOM, which is the *view* of your app.

### Reduce element count

When manipulating the DOM, the secret to getting things done faster is to do less work. This sounds pretty obvious but if you’re coming from a background that stressed SEO and semantic HTML, you might have some habits that introduce unnecessary complexity into the DOM.

Here’s an example that simplifies the markup for a clickable item in a list. While this is a rather simple example, you can imagine places in your code where you can remove or combine unnecessary elements to simplify the DOM.

```html
<ul>
	<!-- Two elements -->
	<li><a href="nextPage.html">Next Page</a></li>
	<!-- One element = less layout, simpler DOM -->
	<li aria-role="button" onclick="goToNextPage()">Next Page</li>
</ul>
```

### Avoid layout thrashing

Certain operations invalidate the layout of the page and require a recalculation. Layout thrashing occurs when you’re reading and updating properties that require a layout before you can read the next property. You might encounter this if you’re looping through a list of nodes and changing how they’re displayed. If you depend on getting values from the DOM, get all those values before you begin to update the DOM. Here is a bad example followed by a corrected good example that demonstrates changing a list of items to absolute positioning.

```javascript
// BAD
// Absolutely position elements, thrashing the DOM
// by repeatedly reading offsetLeft then changing the value of left
for (var i = 0; i < elements.length; i++) {
	elements[i].style.left = elements[i].offsetLeft + " px";
	elements[i].style.top = elements[i].offsetTop + " px";
	elements[i].style.position = "absolute";
}

// GOOD
// Use two loops and two arrays to store the values
// to avoid the repetitive read/update cycle
var leftPx = new Array(elements.length);
var topPx = new Array(elements.length);
for (var i = 0; i < elements.length; i++) {
	leftPx[i] = elements[i].offsetLeft;
	topPx[i] = elements[i].offsetTop;
}
for (var i = 0; i < elements.length; i++) {
	elements[i].style.left = leftPx[i] + "px";
	elements[i].style.top = topPx[i] + "px";
	elements[i].style.position = "absolute";
}
```

### Virtualize lists

If you are displaying long lists in your app, you should consider virtualizing the list. Virtualization aims to keep in the page only the elements that appear on screen, drastically reducing the number of elements in the DOM. In addition to reducing DOM complexity and therefore speeding up DOM modifications, it also reduces memory overhead. In one of our tests with a very large list, virtualization reduced memory usage to nearly a quarter of the non-virtualized usage, a very important optimization for low-memory mobile devices.

While code demonstrating virtualization would be a bit much for this post, if you’re using a UI framework with data binding, there’s a good chance that it provides a way to virtualize lists. In our tests, we used WinJS and it performed incredibly well.


## Images

Generally, there isn’t much you can do with images. Be aware that it takes time to decode an image and it takes memory to keep it on the page, so be sure you have the right resolution images for the device so that you aren’t wasting resources scaling down images that are excessively large.

### Gradients vs. Images

Although CSS gradients have universal support now, our tests showed that it’s faster to change the size of an element with a background provided by an image gradient than a CSS gradient. While this might not move your app into higher interaction classes by itself, if you’re looking to shave off milliseconds and you’re animating elements with CSS gradients, try benchmarking against an image gradient and see if that helps.

## Animations

With animations, it is critical to make sure that they fall into the fluid interaction class. If the time needed to update the page as it draws each frame in the animation exceeds 17 milliseconds, your animation won’t be perfectly smooth.

### Transform vs. position

One of the tricks to getting high performance animations is to get the animation to happen on the GPU. Rather than animating the position by changing top and left via keyframes, you should use the transform property to translate the element instead. This keeps the browser from having to invalidate and recalculate the layout, and instead, allows it to use the GPU to just move the layer holding the element you are animating. Here’s some CSS that shows bad and good ways to define the keyframes.

```css
/* Bad */
@keyframes backAndForth {
    0% { left:   0;   }
   50% { left: 100px; }
  100% { left:   0;   }
}
/* Good */
@keyframes backAndForth {
    0% { transform: translateX(0);     }
   50% { transform: translateX(100px); }
  100% { transform: translateX(0px);   }
}
```


### Using CSS vs. JS for animations

Although today, CSS will promise the fastest, smoothest animations, there are cases where you’ll need to use JavaScript. If you need your animation to be interactive, like moving an object to a user-defined position or dynamically pausing or changing, you may need to consider JavaScript to run the animation. Use ```requestAnimationFrame``` to do the work of scheduling the callbacks for the smoothest and fastest animations.

```javascript
var move = function () {
  someFunctionToMoveTheElement();
  if (isAtDestination() == false) {
    requestAnimationFrame(move);
  }
}
requestAnimationFrame(move);
```


## Memory Management and Garbage Collection

In traditional application models, every navigation tears down your page and rebuilds it on the next page. This is the most powerful option for garbage collection because you don’t need to worry much about garbage collection if you can just reset everything ever minute or so. However, most likely, your Cordova app is built with a single page architecture, meaning it could be around for hours or even days. This makes paying attention to leaking memory important.
Additionally, garbage collection in JavaScript isn’t very predictable and can be quite slow, meaning that the animation that you perfected with our earlier advice now stutters when the garbage collection happens.


### Event Listeners

Here is an example that, if you run it, every second adds 100 buttons to the page and then clears them a half second later. If you watch the memory usage for this, you’ll see it gradually go up over time until the garbage collector kicks in and removes everything. Part of the reason for the increase is that our clear method just sets the HTML to an empty string and it takes a while for the garbage collector to notice our event listeners are hanging around.

```javascript
var container = document.body;
// Remove the buttons by just setting the HTML to nothing
function clear() {
  container.innerHTML = "";
}
function clickHandler() {
  alert('You clicked my button');
}
// Append one button to the page
function appendOneButton() {
  var btn = document.createElement("button");
  btn.setAttribute('value', 'My Button');
  btn.addEventListener('click', clickHandler);
  container.appendChild(btn);
}
// Append n buttons to the page
function appendButtons(n) {
  for (var j = 0; j < n; j++) {
    appendOneButton();
  }
}
// Every second, append 100 buttons and clear them 500ms later
setInterval(function () {
  appendButtons(100);
  setTimeout(clear, 500);
}, 1000);
```
A more memory efficient way to clean up the buttons would be like this:

```javascript
function clear() {
  var children = container.childNodes;
  for (var i = 0; i < els.length; i++) {
    children[i].removeEventListener('click', clickHandler);
    container.removeChild(children[i]);
  }
}
```
The takeaway here is either to be proactive, and clean up as you go, or to be patient and wait for the garbage collector to do its job.

### Event Bubbling and Shared Listeners

It is super common to see code that follows this pattern:

```javascript
for(elem in elements) {
  elem.addEventListener("click", function () {
    /* do some work */
  });
}
```

Adding an anonymous function as the event listener for every element ends up allocating a bunch of excess memory for functions that are identical. There are two better options here. First, attach event listeners to every item by using a function that is saved to a variable rather than an anonymous function. Another one is to attach a single event listener to a parent in the DOM tree and let event bubbling carry the event fired on the child up to the parent.


### Memory Leaks

If you’re waiting around for the garbage collector to clean up all your unused memory, it’s important to make sure that your app isn’t accidently hanging on to elements you expect to get cleaned up. The effect of this is compounded in a single page applications.
For example, assume that your single page app has containers for every page that are created, populated and removed as you navigate through it. You might have a line like this, where you create a local variable to hold a reference to your container:

```javascript
var homeContainer = document.getElementById("homeContainer");
```

This is a trivial typo that can easily slip through code review, simply dropping the var from the beginning of the line, will cause ```homeContainer``` to be added as a property on the window. This is innocent enough, but now when your app navigates to a new page and cleans up the DOM by removing ```homeContainer```, your window maintain a reference to the element and prevent it from getting garbage collected!
If you use the memory profiler in your browser’s developer tools, these elements should be identified as “Detached DOM Tree” nodes, making them easy to identify if you are leaking this way.


### External Links

Below are some links for further reading on performance improvements in Cordova apps:

- Christophe Coenraets' [PhoneGap Performance slides][coenraets]
- Ryan Salva's [Cordova Performance presentation][salva]

## Improving native code, in Objective-C/Swift, Java, or WinJS (Cordova plugin code)

In a Cordova app, all code that will usually be written is in JS, CSS, and HTML. However, the source for native Cordova components (like the plugins and the runtime) is open, and it is within the power of an app developer to change it. Optimizing platform-specific code is outside the scope of this document, but many guides are available online for each platform supported by Cordova. They are as follows:

- [Amazon FireOS] [perf_amazon_fireos]
- [Android] [perf_android]
- [BlackBerry 10] [perf_blackberry]
- [iOS] [perf_ios]
- [Tizen][perf_tizen]
- [WinJS] [perf_winjs]
- Browsers: [Chrome][perf_chrome], [Firefox][perf_firefox], [Safari][perf_safari], [Opera][perf_opera]

## Navigating the Web-to-Native Bridge (Cordova runtime code)

Between the web code and native code is the Cordova runtime JavaScript and native code, which is also freely available and can be modified by any developer through the same process as is used to modify plugins. All Cordova repositories are available on GitHub under the [Apache organization][cordova_repos], prefixed with the string `cordova-`.

[perf_amazon_fireos]: https://developer.amazon.com/appsandservices/community/post/TxSKXI5UIOVFFU/Build-Higher-Performance-Cordova-Based-Fire-OS-Apps-by-Implementing-Amazon-Web-V
[perf_android]:       http://developer.android.com/training/articles/perf-tips.html
[perf_blackberry]:    https://developer.blackberry.com/native/documentation/best_practices/performance/performance.html
[perf_tizen]:         https://developer.tizen.org/documentation/guides/web-application/w3chtml5supplementary-features/performance-and-optimization
[perf_winjs]:         https://msdn.microsoft.com/en-us/magazine/dn574803.aspx
[perf_ios]:           https://developer.apple.com/library/mac/documentation/Performance/Conceptual/PerformanceOverview/BasicTips/BasicTips.html

[perf_chrome]:  http://addyosmani.com/blog/performance-optimisation-with-timeline-profiles/
[perf_firefox]: https://developer.mozilla.org/en-US/docs/Mozilla/Performance
[perf_opera]:   http://google.com
[perf_safari]:  http://google.com

[coenraets]:       http://coenraets.org/keypoint/phonegap-performance/#0
[salva]:           https://channel9.msdn.com/Events/Build/2015/3-756
[fastclick]:       https://github.com/ftlabs/fastclick
[mdn_transitions]: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Using_CSS_transitions
[cordova_repos]:   https://github.com/apache/?utf8=%E2%9C%93&query=cordova-
