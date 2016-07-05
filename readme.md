# Events and Callbacks

## Screencasts

* [Robin's Screencast](https://youtu.be/S4Xvo_m6P04)
* [Andy's Screencast - p1](https://www.youtube.com/watch?v=xogI6prB-PI)
* [Andy's Screencast - p2](https://www.youtube.com/watch?v=Srd2Tx1Z7v8)

## Learning Objectives

* Explain the concept of a 'callback' and how we can pass functions as arguments to other functions.
* Explain why callbacks are important to asynchronous program flow.
* Pass a named function as a callback to **another** function.
* Identify when to **reference** a function and when to **invoke** a function.
* Describe what an anonymous function is and when you would use one.
* Pass an anonymous function as a callback to another function.
* Define what `this` represents in the context of an event listener.
* Describe the difference between asynchronous and synchronous program execution.

## Framing (2:30 - 2:35, 5 minutes)

In order to do things on the client side and give our web applications behavior, we need programmatic access to the HTML and CSS using Javascript. Javascript gives us not only the ability to manipulate the **DOM** as we've seen, but also to make it respond to user actions. This is where events come in: we can *listen* for certain kinds of user-driven events, such as clicking a button, entering data into a form, keypresses, and many, many more.


## User Interaction

As we write client-side Javascript (javascript that is executed by *our browsers*, as opposed to being executed by a server we are accessing), it is very important to keep the user's actions in mind when designing our app's UI.

For example, let's say we have a single button on our landing page, we need to write some code that will execute whenever a user clicks on that button, i.e. pop-up an ad with a special one-and-a-lifetime promotion for that user.

## What is Asynchronicity?

Javascript typically will run top-to-bottom, however, we as developers have no idea when the code related to the button click will actually be executed, it's totally dependent on the user. Therefore, we need to write code that will execute asynchronously, and not hold up the rest of our application.

Javascript as a language was built with this problem in mind and has spawned many more solutions that have been introduced through libraries, packages, and frameworks. Together, these tools provide some abstractions to interact with events. As such, we as developers can write code to listen for and respond to these events.

Today, we will get practice writing the underlying code responsible for adding behavior to a webpage with jQuery.

## jQuery and Vanilla Javascript

Note that we are using jQuery for this lesson; one thing to note about jQuery is that it is a **library** written in javascript that allows us to do more with less code. There may be situations where we'd want to use vanilla javascript over jQuery to write our code; these might include reverse compatibility (older versions of IE) and potentially performance. Check out [this](http://youmightnotneedjquery.com/) website for side-by-side comparisons of jQuery and vanilla javascript.

Interspersed throughout the lesson are examples in vanilla javascript that are equivalent to jQuery implementations.

In general, mixing vanilla javascript expressions with jQuery will usually result in errors. Stick with jQuery for now.

## Set Up (2:35 - 2:40, 5 minutes)

For this lesson we'll be working with only two files: `index.html` and `script.js`. Create these files in your a folder in your sandbox directory...

```bash
$ cd ~/wdi/sandbox
$ mkdir events-callbacks-practice
$ cd events-callbacks-practice
$ touch index.html script.js
```

Next, set up your `index.html` file, making sure to include a button, a link to `script.js` and load in jQuery...

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
  <head>
    <title>Events and Callbacks Practice</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>
  </head>
  <body>
    <button>Click me!</button>
    <script src="script.js"></script>
  </body>
</html>
```

Now let's put a simple block of code in `script.js` to make sure it's properly linked to `index.html`...

```js
// script.js
$(document).on("ready", function() {
  console.log( "The page's contents have finished loading!" );
})
```

#### `$(document).ready()`

In order to use jQuery to select elements and add event listeners, we need to make sure that the **page's content is fully loaded** and the page is "ready" for DOM traversal and manipulation.

### You Do: What Is An Event? (2:40-2:45, 5 minutes)

But first, a question for you: **What is an event?** Spend two minutes doing the following tasks. You are encouraged to discuss your findings with a partner during the exercise.
1. Come up with your own definition without looking at any other sources. Don't worry about getting it right -- what do you **think** an event is?
2. Now, find (i.e., Google) some documentation on Javascript events. Does that information match your definition? How would you change it?
3. Write down three examples of an event.  

> If you need some help, you can find information on events and examples [W3Schools](http://www.w3schools.com/js/js_events.asp) and [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Events).

### Setting Up An Event Listener (2:45-2:55, 10 minutes)

Now that we know a bit about events in Javascript, let's wire up our code to be able to respond to those events. In order to run code in response to an event, we need to define an **Event Listener**. Below you'll find a simple event listener. It's purpose? Print a message to the console whenever a button is clicked...

#### in `script.js`:

```js
var button = $("button");

function handleClickEvent(){
  console.log( "I was clicked!" );
}

// This is the event listener!
button.on( "click", handleClickEvent );
```

Let's go through the above code examples line-by-line...

#### Selecting the Element

We want our "click handler" -- what we're calling an event listener -- to trigger every time the button is clicked. In order for this to happen, we need to select the button from the DOM and represent that button in Javascript.

<details>
<summary>
Selecting DOM elements with Vanilla JS & jQuery
</summary>
In vanilla javascript, we would use `.querySelector()`, however in jQuery we ***wrap it in ca$h*** using the `$()` function which has some added functionality than its vanilla js counterpart. If `.querySelector()` were a kitchen knife, `$()` would be a swiss army knife or a multi-tool.
</details>

```js
var button = $( "button" );
```
> `.$()` selects all HTML elements matching the passed-in argument. NOTE: `$()` will return whatever it selects **in a special jQuery array-like object**. In the above example, that is `<button>`.  You can also pass in a class (ex. `$(".exampleClass")`). With an id  (ex. `$("#exampleId")`), `$()` will select the first id that matches.  

> To deal with jQuery `$()` returning things in an array, we have a special jQuery method called `.eq()`. Do not use `[]`, use `.eq()` instead.

Now the `button` variable contains a reference to the button that exists on our page.  

#### Defining the Behavior

When our button is clicked, we want some Javascript code to run that prints a message to the console. We are going to **encapsulate** that code in a function. We'll do something with it later.

```js
function handleClickEvent(){
  console.log( "I was clicked!" );
}
```

#### Creating the Event Listener

Now for the big step: linking our behavior with a button click. Let's look at that event listener again and go over each component...

```js
button.on( "click", handleClickEvent );
```

<details>
<summary>Vanilla JS implementation</summary>

```
document.querySelector("button").addEventListener("click", handleClickEvent)

```
</details>

##### `button`

The first component is the HTML element we are applying the listener to. In this case, that is the `<button>`, which is stored in our `button` variable.

<details>
<summary>Vanilla JS implementation</summary>

```
var button = document.querySelector("button");

```
</details>

##### `.on`

Next is the jQuery method that allows us to create event listeners: `.on()`. It takes two arguments...


<details>
<summary>Vanilla JS implementation</summary>

```
.addEventListener()

```
</details>


##### `"click"`

The first argument is where we indicate what type of event we are listening to. In this case, that is a "click". We could replace this with the examples we mentioned earlier in class, like `"submit"` or `"mouseover"`.

##### `handleClickEvent`

The second argument is where we indicate what we want to happen once the event occurs. This is what is known as a **callback**.  

This might not be your first time hearing it, and definitely won't be your last. A callback is a piece of executable code that is passed as an argument to other code, which is expected to invoke (or "call back") that executable code at some convenient time.

The invocation may be immediate or it might happen later. In the example above, `handleClickEvent` is our callback. The invocation happens when the button is clicked.  

> We can also pass in anonymous functions (i.e., define a nameless function directly inside of the event listener) instead of previously-defined functions.

### You Do: Practice (2:55 - 3:05, 10 minutes)

Visit this [repository](https://github.com/ga-wdi-exercises/event-listener-practice.git) and follow the instructions.

### Before We Go On... (3:05 - 3:10, 5 minutes)

Usually when we do anything with functions, we put parentheses after the function name. Here, we have `handleClickEvent` without any parens.

Try adding parentheses at the end of this line:

```js
button.on("click", handleClickEvent() );
```

Let's refresh our pages and then discuss the result. What was different? Why?

> You'll notice that "I was clicked!" pops up immediately upon reload. Also note that the event while it does fire, isn't doing anything. When we include `()` we invoke the function expression. Without the `()`, we're using the function expression as a reference.


## Break (3:10 - 3:20, 10 minutes)

### You Do: Color Scheme Switcher (3:20 - 3:50, 30 minutes)

Clone this repo and follow the readme instructions: **[Color Scheme Switcher](https://github.com/ga-dc/color-scheme-switcher)**.


### `$(this)` (3:50 - 3:55, 5 minutes)

In programming, we will see the keyword `this` quite a bit, especially when we get to object-oriented programming. It's closely related to the idea of **scope**, or where we are in a program during its execution. This will be covered in  detail during the lesson on scope and closures.

With that in the back of our minds, let's have a look at how we can use `this` or rather, `$(this)` in jQuery.

Back in the code we were using in-class...

```js
var button = $("button")
var handleClickEvent = function(){
  console.log("I was clicked!")
}
button.on("click", handleClickEvent);
```

Try this-- insert the following line anywhere in our click-handler:
* `console.log($(this))`
* Now what do you see when we click the button? How would you define `this` in the context of an event listener?

In the context of an event listener callback, `$(this)` always refers to the object that triggered the event.

### You Do: `this` Practice (3:55 - 4:05, 10 minutes)

Clone and follow the instructions in this [repository](https://github.com/ga-wdi-exercises/events-this-practice).

<details>
<summary>
Bonus: `this` and Color Scheme Picker
</summary>
<p>
If you're curious, here's a short solution to the earlier Color Scheme Picker exercise that makes use of `this`. Keep in mind, this is advanced! You are not expected to be able to write code like this yet.  
</p>
`
var buttons = $("li").on("click", function () {
  $("body").attr("class", $(this).attr("class"));
});
`
</details>

### Event Defaults (4:05 - 4:10, 5 minutes)

Back in the code we were using in-class, replace your button with a link to Google...

```html
<body>
  <a href="http://google.com">Click me!</a>
</body>
```

Now, add an event listener to that link that brings up a `prompt` box, asking the user if they want to go to Google...

```js
var link = $("a")
var handleClickEvent = function(e){
  var input = prompt("You sure you want to go to Google?")
}
link.on("click", handleClickEvent);
```

The problem is we don't know how to stop them from going to Google! They go anyway, whether they hit "OK" or "Cancel".

Some elements, like `<a>`, have a default action they perform. In this case, that action is "going to another webpage." You can prevent that default action with an Event property called `preventDefault`.

```js
var link = $("a")
var handleClickEvent = function(e){
  e.preventDefault();
  var input = prompt("You sure you want to go to Google?")
}
link.on("click", handleClickEvent);
```

Now, no matter what the user clicks, they won't go to Google.

In order to make it so they that **do** go to Google on clicking OK, but **don't** on clicking 'Cancel', we can use the fact that when you click 'Cancel' on a `prompt`, it returns `null`...

```js
var button = $("a")
var handleClickEvent = function(e){
  if(prompt("You sure you want to go to Google?") === null){
    e.preventDefault();
  }
}
button.on("click", handleClickEvent);
```

### The Event Object (4:10 - 4:15, 5 minutes)

Now, you're going to make a small change by adding an argument to the anonymous function and printing it to the console...

```js
var button = $("button");
var handleClickEvent = function(evt){
  console.log("I was clicked!")
  console.log(evt)
}
button.on("click", handleClickEvent);
```

The `evt` stands for `event`.
> The reason we're not actually using `event` is that it's a "reserved word" in Javascript, like "if" and "return".

#### You Do: Explore The Event Object (4:15 - 4:20, 5 minutes)

With your partner, spend two minutes clicking the button and exploring what properties the event (or `evt`) object contains. Look for...

* A way to figure out what element was clicked on.
* A way to tell the position of the mouse when it clicked.
* One other piece of useful or interesting information.

### Key Events (4:20 - 4:35, 15 minutes)

Let's explore some other events. Add a text input field into the HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Events and Callbacks Practice</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>
  </head>
  <body>
    <button>Click me!</button>
    <input placeholder="Type here!" />
    <script src="script.js"></script>
  </body>
</html>
```

#### You Do

With a partner, add an event listener for the `keyup` event to the input. Explore the `event` object again. **Can you find a way to tell which key was pressed?**

#### We Do

Your code in `script.js` should look something like this...

```js
var button = $("button");
var input = $("input");
var handleClickEvent = function(evt){
  console.log("I was clicked!")
  console.log(evt)
}
var handleKeyboardEvent = function(evt){
  console.log("You used the keyboard!")
  console.log(evt)
}
button.on("click", handleClickEvent);
input.on("keyup", handleKeyboardEvent);
```

A cross-browser way of telling which key is pressed is using the `keyCode` property. For `d`, `evt.keyCode` is `68`. For Shift, it's `16`.

#### You Do

Find the keyCodes for...
* Enter
* Tab
* Delete

#### You Do

There are several other events that come up with the `input` tag. See if you can figure out the difference between:

* `keyup`
* `keydown`
* `keypress`
* `change`
* `focus`
* `blur`

There are a bunch of different browser events you can use in Javascript, all [listed at W3Schools](http://www.w3schools.com/jsref/dom_obj_event.asp).

> Some programmers have qualms with W3Schools since they're mooching off the name of the W3 without actually being related to them. However, this list is accurate and easy-to-read.

## Break (4:35 - 4:45, 10 minutes)

## Timing Functions (4:45 - 4:55, 10 minutes)

Let's look at timing functions -- that is, Javascript's way of making something happen every `x` seconds.

Replace the contents of your `script.js` with this:

```js
function sayHello(){
  console.log("Hi there!")
}
setInterval(sayHello, 1000);
```

### Turn and Talk

* What does the number in `setInterval` indicate?
* Replace `setInterval` with `setTimeout`. What's the difference?

We'll make it more interesting by having the timer start on a click event, and stop on another click event.

Put a "start" and a "stop" button in your HTML...

```html
<button id="start">Start</button>
<button id="stop">Stop</button>
```

Then, replace the contents of your `script.js` with this...

```js
var start = $("#start");
var stop = $("#stop");
var singAnnoyingSong = function(){
  console.log("I know a song that gets on everybody's nerves...")
}
var songTimer;
start.on("click", function(){
  songTimer = setInterval(singAnnoyingSong, 100);
});

stop.on("click", function(){
  clearInterval(songTimer);
});
```

### Turn and Talk

* What happens when you click the "start" button a bunch of times in a row?
  * Why?
  * How is this different from events?
  * When you do this, why doesn't the "stop" button seem to work?
* What does `clearInterval` do?
* Give the anonymous function callbacks an argument of `evt`, like we did for the event listeners, and print it to the console. What information does it contain?


## Asynchronicity (4:55 - 5:00, 5 minutes)

Run the next bit of code and you can see asynchronous program execution.

```js
function anAsyncFunction(){
  console.log("hello")
  setTimeout(function(){
    console.log("this is happening in the middle")
  }, 5000)
  console.log("goodbye")
}

anAsyncFunction();
```

Wait, what? The goodbye came before the "this is happening in the middle"!

With everything else we've seen, Javascript executes one line of code, then when that line is done, executes the next line of code. This is called being **synchronous**.

However, some operations in Javascript are **asynchronous**, meaning Javascript goes on to the next line of code without waiting for the previous line to complete.

This is limited mostly to timing functions, and operations where Javascript is loading data from some other website.

### Why doesn't Javascript wait for these operations to complete before going to the next line of code?

Because otherwise the webpage would just "hang" until the operation completes. The browser can't do anything while Javascript is actively running. We've seen this when we've encountered infinite `while` loops. Asynchronicity is a way of preventing the computer from freezing.

This risk is greatest when Javascript is making requests to other webpages. There's no way of knowing how long the request will take to complete. It could be near-instant, but if the target server is having a bad day, it could take who-knows-how-long. You don't want the operability of your computer to be at the mercy of some random computer somewhere else.

In this small app we made, anything we want to be sure happens **after** those 5 seconds of computing should go inside the callback of the `setTimeout`. This way, we can be certain that it will run only when the 5 seconds are up.

## Sample Quiz Questions

1. What is the difference between synchronous and asynchronous program execution?
2. Define a function that takes a function as an argument and invokes the argument when the function is called.



## Additional Practice

* [Timer JS](https://github.com/ga-wdi-exercises/timer_js) (Practice with Timers)
