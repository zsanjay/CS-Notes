
https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Events

https://developer.mozilla.org/en-US/docs/Web/Events

# The addEventListener() Method

The JavaScript addEventListener() method allows you to set up functions to be called when a specified event happens, such as when a user clicks a button. This tutorial shows you how you can implement addEventListener() in your code.

##Understanding Events and Event Handlers

**Events** are actions that happen when the user or browser manipulates a page. They play an important role as they can cause elements of a web page to change dynamically. 

For example, when the browser finishes loading a document, then a `load`event occurred. If a user clicks a button on a page, then a `click` event has happened.

Many events can happen once, multiple times, or never. You also may not know when an event will happen, especially if it is user generated. 

In these scenarios, you need an **event handler** to detect when an event happens. This way, you can set up code to react to events as they happen on the fly.

JavaScript provides an event handler in the form of the `addEventListener()` method. This handler can be attached to a specific HTML element you wish to monitor events for, and the element can have more than one handler attached.

##addEventListener() Syntax

Here's the syntax:

```js
target.addEventListener(event, function, useCapture)
```

- **target**: the HTML element you wish to add your event handler to. This element exists as part of the Document Object Model (DOM) and you may wish to learn about [how to select a DOM element](https://1000mileworld.com/dom-manipulation-using-javascript/#select).
- **event**: a string that specifies the name of the event. We already mentioned `load` and `click` events. For the curious, here's a full list of [HTML DOM events](https://www.w3schools.com/jsref/dom_obj_event.asp).
- **function**: specifies the function to run when the event is detected. This is the magic that can allow your web pages to change dynamically.
- **useCapture**: an optional Boolean value (true or false) that specifies whether the event should be executed in the [capturing or bubbling phase](https://javascript.info/bubbling-and-capturing). In the case of nested HTML elements (such as an `img` within a `div`) with attached event handlers, this value determines which event gets executed first. By default, it's set to false which means that the innermost HTML event handler is executed first (bubbling phase).
- 

This is a simple example I made which shows you `addEventListener()` in action.

When a user clicks the button, a message is displayed. Another button click hides the message. Here's the relevant JavaScript:

```js
let button = document.querySelector('#button');
let msg = document.querySelector('#message');

button.addEventListener('click', ()=>{
  msg.classList.toggle('reveal');
})
```

Going by the syntax shown previously for `addEventListener()`:

- **target**: HTML element with `id='button'`
- **function**: anonymous (arrow) function that sets up code necessary to reveal/hide the message
- **useCapture**: left to default value of `false`

My function is able to reveal/hide the message by adding/removing a CSS class called "reveal" which changes the message element's visibility.

Of course in your code, feel free to customize this function. You may also replace the anonymous function with a named function of your own.

##Passing Event as a Parameter

Sometimes we may want to know more information about the event, such as what element was clicked. In this situation, we need to pass in an event parameter to our function. 

This example shows how you may obtain the element's id:

```js
button.addEventListener('click', (e)=>{
  console.log(e.target.id)
})
```

Here the event parameter is a variable named `e` but it can be easily called anything else such as "event". This parameter is an object which contains various information about the event such as the target id.

You don't have to do anything special and JavaScript automatically knows what to do when you pass in a parameter this way to your function.

##Removing Event Handlers

If for some reason you no longer want an event handler to activate, here's how to remove it:

```js
target.removeEventListener(event, function, useCapture);
```

The parameters are the same as `addEventListener()`.

```js
document.addEventListener("readystatechange" , (event) => {

if(event.target.readyState === "complete") {

console.log("readyState : complete");

initApp();

}

})

  

//Event Bubbling

const initApp1 = () => {

const view = document.querySelector('#view2');

const div = view.querySelector("div");

const h2 = div.querySelector("h2");

  

view.addEventListener("click" , (event) => {

//event.target.style.backgroundColor = "purple";

// view.classList.remove("darkblue");

// view.classList.add("purple");

view.classList.toggle("darkblue");

view.classList.toggle("purple");

});

  

div.addEventListener("click" , (event) => {

//event.target.style.backgroundColor = "blue";

div.classList.toggle("black");

div.classList.toggle("blue");

});

  

h2.addEventListener("click" , (event) => {

//event.stopPropagation();

event.target.textContent = event.target.textContent === "My 2nd View" ? "Clicked" : "My 2nd View";

});

  

const nav = document.querySelector("nav");

nav.addEventListener("mouseover", (event) => {

event.target.classList.add("height100");

});

nav.addEventListener("mouseout" , (event) => {

event.target.classList.remove("height100");

});

}

  

const initApp = () => {

const view3 = document.querySelector("#view3");

const myForm = view3.querySelector("#myForm");

  

myForm.addEventListener("submit" , (event) => {

event.preventDefault();

console.log("submit event");

});

}
```

### References

https://www.youtube.com/watch?v=UVRDq-wnfgk&list=PL0Zuz27SZ-6Oi6xNtL_fwCrwpuqylMsgT&index=22

https://www.freecodecamp.org/news/javascript-addeventlistener-example-code/

