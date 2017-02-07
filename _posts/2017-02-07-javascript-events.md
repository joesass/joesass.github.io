---
layout: post
title: Javascript Events
---


When I started learning Javascript, I, like you, thought Events were going to be fun!! After all, who doesn't like to party?

Boy was I wrong.

![It will be fun they said](http://i.imgur.com/LwQIwLX.jpg)

Events are anything but a party, to understand and to implement, and besides, I never seem to get invited to the cool ones.

So as some form of revenge I decided that I would try to understand them and what makes them tick, so that I can be one of the cool kids again.

## Why are Events important?

One of the main reasons javascript was invented and added to the browser is because we wanted to give the user a way to interact with our plain old HTML.

We had a problem though.

How are we supposed to know when a user does something?

Well, we do have the user's input devices(keyboard, mouse, etc.) so why not receive input from those devices and then so stuff with that.

How will we get that information from the input devices?

We can keep asking the inputs if they have anything for us, but we would have to do this many times a second for every input device. This technique is called *polling*, and as you can imagine it is not the most efficient solution.

Also, what if the user doesn't do something right away, but rather after a little while, like let's say click a button to say they liked an article. Or what if there were 5 different clicks on different parts of the page, or 3 keys being pressed at once, how do we handle this information?

Also, what if want to do something in our program based on an input, but that thing takes us a long time. Should we stop the page completely until the task finishes? What if the user wants to continue reading the page or interacting with it somewhere else?

## Asynchronicity and Events

In order to solve these problems, javascript has two ideas that play into each other. One is asynchronicity, which means that our code does not execute in a straight line, but rather gets piled onto a big `event loop` or queue and gets executed when its' the first in line.

For example, we have something called `setTimeout` which takes in a function and a number of milliseconds, and adds that function to the event loop to be executed after those milliseconds elapse.

Similarly we can tell our program that we want to add a certain function to be executed when it receives a specific event from a certain part of our page.

Events are then added to a queue, which is how the brits say line, and we can execute our functions in order that the events were received in the queue. This way we don't need to listen for every single user input but rather wait for it in our queue. Neat!


## Event Types

At it's core an Event is simply an object.

What type of object?

A Javascript object.

They are nothing special. In fact you can make one by using the `new` keyword just like any other object.

```js
const partay = new Event('click')

console.log(partay)
// -> Event { isTrusted: false, type: 'click', etc.}
```

Then you can `dispatch` the event using `dispatchEvent()`

```js
document.dispatchEvent(partay)
// -> true
```

At this point nothing will happen because we haven't told our program to listen for this type of event.

We can do this by adding a listener to a certain element on our page.

This will add the particular Event object to the `event loop`

This object is being passed into our program either through the DOM(most of the time) or through our code, and that notifies our program that we should do something now.

## Listeners

There are events happening on our page every second, every time someone hovers over an element, clicks their mouse or even moves there mouse, or presses a key on their keyboard, a message is being sent through the DOM into our program with a whole bunch of information attached.

So why doesn't our program do anything with all this information? Why are we not constantly seeing changes to our page?

The answer is Event Listeners. What these do is tell our page, or an element in our page to *listen* for a certain event, and then do something when it receives that message.

Continuing our example above, we can add an event listener by calling the `addEventListener()` function provided by javascript on an element of our page and passing in the event type i.e. `click` and the function that we want to execute when we do receive that event, also known as a callback function.

## Handlers

What should we do when we receive one of these Events? Well that's what the EventHandler is for. After telling our element to listen for an event we want to tell it what to do, and we do that in the form of passing in a function as the second argument to the listener function.

```js
document.addEventListener('click', () => {console.log('clickeroooo') })
```

Now when we dispatch our click event, our event will get added to the event loop and will get called when it is first in line.

```js
document.dispatchEvent(partay)
// -> clickeroooo
```

## Bubbling

![Bubbles](http://i.giphy.com/8FpRXTv389suQ.gif)

One of the interesting things about Events is that they bubble up through the DOM. What this means is that if we have a nested element like this:

```html
<div id="parent">
  <div id="child"></div>
</div>
```

And we add a click handler to both of them:

```js
document.getElementById('parent').addEventListener('click', () => { console.log('I am your faaaaather') })
document.getElementById('child').addEventListener('click', () => { console.log('Noooooooo') })
```

[Try this example on Codepen](http://codepen.io/dibson/pen/vgaMvR?editors=1111)

When someone clicks on the child element, the parent element will also register a click and fire it's callback. This is a property that can be set when creating an event and by default on a click event it's set to true.

## Capturing

Another interesting thing about events is that they don't actually originate where they occurred. They always start at the root of the document and then look down through the DOM tree in order to determine what the target of the event was. This process is called capturing. In fact the bubbling itself goes all the way back to the root of the document and fires the handlers for all nodes that have a listener.

## Target and CurrentTarget

We mentioned a target, but what does this mean?

Simply put, a target is the node from which the event originated. As we saw before, this is not a trivial thing for the DOM to figure out, as all events fundamentally start at the root and are captured later on. When the DOM does find out who triggered it, it sets that element as the `target` property of the event. Another useful property of an event is the `currentTarget` property which is useful when an event is bubbling through the DOM. It will give us the element that is currently calling its listener.

## Default Behaviors

There are default functions that fire for certain events. For example, if someone clicks a link, the browser will redirect to that URL and send off a request. Similarly, when someone submits a form, the browser will send a POST request with the inputs as params. In order to stop this behavior we need to specify that want to `preventDefault()` in our handler callback.



Cool Links:

- [Event Capturing and Bubbling](https://www.kirupa.com/html5/event_capturing_bubbling_javascript.htm)
- [MDN Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
- [Eloquent Javascript](http://eloquentjavascript.net/14_event.html)
