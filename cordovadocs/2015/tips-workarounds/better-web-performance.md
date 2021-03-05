--- 
description: "When you build a Cordova app, you'll write the User Interface (UI) and the majority of the application’s logic by using web technologies."
title: "Optimize the performance of a Cordova app"
author: "jmatthiesen"
ms.prod: "visual-studio-dev14"
ms.topic: "troubleshooting"
ms.date: "10/08/2015"
ms.author: jomatthi
--- 

# Optimize the performance of a Cordova app

When you build a Cordova app, you'll write the User Interface (UI) and the majority of the application’s logic by using web technologies. This most likely includes a combination of HTML, JavaScript and CSS. To build Cordova apps that perform well, it’s important to understand how to use these technologies in the most efficient ways to obtain the best performance. This topic surveys a variety of techniques that you can apply including Document Object Model (DOM) complexity, animation techniques, and memory management.

## Interaction Classes

Before we get into specific tricks, it’s important to understand how users react to having to wait for actions in your app to complete. Users are impatient and expect your app to perform. These expectations can be grouped into the following categories:

* Fluid
* Instantaneous
* Fast
* Responsive

### Fluid (17ms)

The majority of screens today refresh at 60 Hz, that is, 60 frames per second. This is around the upper limit of what the eye can perceive. If an operation takes less than one frame, or roughly 17 milliseconds, it won’t interrupt fluid interactions such as scrolling or animations. This “lag free” behavior is the holy grail of app performance and delights users.

### Instantaneous (17-100ms)

The next interaction class ranges from 17 to 100 milliseconds. Operations in this span might be perceptible to the user, but finish so quickly that they will barely register the pause. For example, to have a button instantly respond to a press, a response as slow as 100ms might feel immediate to the user. This response time may be too slow though for dragging items around on the screen and having the items feel “stuck” to the user’s finger.

### Fast (100-250ms)

For something to feel fast, it should probably fall into the range of 100-250ms. This could be navigation between simple pages or loading an image that is already present on the device. This response time will likely be too slow to allow the latency required when fetching resources from the network, especially on a cellular data connection.

### Responsive (250-1000ms)

Under a second, your app will still feel responsive and your users won’t disengage. You might want to display a loading spinner or a progress bar, especially if you know the activity may stretch beyond one second. Within this interaction class, you might be making a network call or crunching some numbers before updating the UI with your results.

## DOM

Nearly everything your app will do interacts with in the DOM, which is the *view* of your app.

### Reduce element count

When manipulating the DOM, the secret to getting things done faster is to do less work. This sounds pretty obvious but if you’re coming from a background that stressed SEO and semantic HTML, you might have some habits that introduce unnecessary complexity into the DOM.

Here’s an example that simplifies the markup for a clickable item in a list. While this is a rather simple example, you can imagine places in your code where you can remove or combine unnecessary elements to simplify the DOM.

```
<ul>
	<!-- Two elements -->
	<li><a href="nextPage.html">Next Page</a></li>
	<!-- One element = less layout, simpler DOM -->
	<li aria-role="button" onclick="goToNextPage()">Next Page</li>
</ul>
```

### Avoid layout thrashing

Certain operations invalidate the layout of the page and require a recalculation. Layout thrashing occurs when you’re reading and updating properties that require a layout before you can read the next property. You might encounter this if you’re looping through a list of nodes and changing how they’re displayed. If you depend on getting values from the DOM, get all those values before you begin to update the DOM. Here is a bad example followed by a corrected good example that demonstrates changing a list of items to absolute positioning.

```
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

```
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

```
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

```
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

```
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

```
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

```
var homeContainer = document.getElementById("homeContainer");
```

This is a trivial typo that can easily slip through code review, simply dropping the var from the beginning of the line, will cause ```homeContainer``` to be added as a property on the window. This is innocent enough, but now when your app navigates to a new page and cleans up the DOM by removing ```homeContainer```, your window maintain a reference to the element and prevent it from getting garbage collected!
If you use the memory profiler in your browser’s developer tools, these elements should be identified as “Detached DOM Tree” nodes, making them easy to identify if you are leaking this way.

## Conclusion

The areas we’ve mentioned in this topic and others in this section of the site are kind of like the classifications you studied in biology: these are just the big families and there are many specific genus and species under each family, each being a little different from the other and coming with their own set of concerns. Hopefully this topic gave you the big picture to identify the families of issues in your own code and help you write more performant code. Know of a family we didn’t cover here or a specific genus or species that really tripped you up? Leave a comment to help others identify all the beasts that inhabit the world of browser performance.
