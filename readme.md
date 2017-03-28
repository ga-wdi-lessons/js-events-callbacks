# Events and Callbacks

## Learning Objectives

* Explain the concept of a 'callback' and how we can pass functions as arguments to other functions.
* Explain why callbacks are important to asynchronous program flow.
* Pass a named function as a callback to **another** function.
* Identify when to **reference** a function and when to **invoke** a function.
* Pass an anonymous function as a callback to another function.
* Define what `this` represents in the context of an event listener.
* Describe the difference between asynchronous and synchronous program execution.

## Framing (5 minutes / 0:05)

So far, we have needed to use the REPL in the browser console to interact with our programs.
This is asking a bit much of our users.
Instead we would like to write code to respond to user interactions with the webpage.
The **DOM** not only lets us manipulate the document or webpage using JavaScript, but also gives us the ability to write JavaScript that responds to interactions with the page.
These interactions are communicated as **events**.

> What might be an event we would want to respond to with JavaScript?

We can *listen* for certain kinds of user-driven events, such as clicking a button, entering data into a form, keypresses and many, many more.

## You Do: What Is An Event? (10 minutes / 0:10)

> 7 minutes exercise. 3 minutes review.

Spend 7 minutes doing the following tasks. You are encouraged to discuss your findings with a partner during the exercise.

1. Come up with your own definition without looking at any other sources. Don't worry about getting it right -- what do you think an event is?
2. Now, find (i.e., Google) some documentation on Javascript events. Does that information match your definition? How would you update your definition to include what you have learned but rephrased in your own words?
3. Write down three examples of events.

> If you need some help, you can find information on events and examples [W3Schools](http://www.w3schools.com/js/js_events.asp) and [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Events).

When you have finished, read [this issue](https://github.com/ga-wdi-lessons/js-events-callbacks/issues/9) and submit a comment with the three parts clearly labeled.

### User Interaction

As we write client-side Javascript -- javascript that is executed in *the browser* -- it is very important to keep the user's actions in mind when designing our app's UI.

For example, let's say we have a single button on our landing page, we need to write some code that will run whenever a user clicks on that button.

### What is Asynchronicity?

In the JavaScript programs we have written so far, code runs in sequence; one line executing after another.
When interpreting it, we start at the top of the file and read line by line to understand expected behavior.
The flow of code goes from top to bottom.
We can invoke a function which has been previously defined but we understand the code of the function body running line by line at the point of invocation.

In the case of our button however, when that code actually runs is totally dependent on the user.
When a user clicks the button an event will be triggered.
Therefore, we need to write code that will execute asynchronously -- kicking off when the user clicks.
We do not want to hold up the rest of our application waiting for the user to click.

Javascript as a language was built with this problem in mind and has spawned many more solutions that have been introduced through libraries, packages and frameworks as well as default language features.
Together, these tools provide some abstractions to interact with events. As such, we as developers can write code to listen for and respond to these events.

Today, we will get practice writing the underlying code responsible for adding behavior to a webpage with jQuery.

> The term asynchronicity, like many in programming may be unduly intimidating. It simply means "not happening at the same time", not a part of this top to bottom flow.

> We have actually seen this at play in our own code regularly when working at the REPL and not found it to be intuitive possibly to the point of being mundane.
> The reason for using terms like asynchronicity is not to sound smart or create a barrier to entry, but so that we can be very exact when we talk about programming.


### jQuery and Vanilla Javascript

Note that we are using jQuery for this lesson. jQuery is a **library** written in javascript that allows us to do more with less code and hides some frustrating or counterintuitive behavior of the DOM.

> What is code that hides unimportant details of something called?

If so inclined we can even look at the [source code on GitHub](https://github.com/jquery/jquery) (took me a while to find the famous `$` but it's [here](https://github.com/jquery/jquery/blob/master/src/exports/global.js#L31)). Keep in mind this is one of the most battle hardened code bases of all time with tens of thousands of lines of code, thousands of commits, and hundreds of contributors.

Check out [this website](http://youmightnotneedjquery.com/) for a more consumable side-by-side comparisons of jQuery and vanilla javascript.

Interspersed throughout the lesson are examples in vanilla Javascript that are equivalent to jQuery implementations.

### We Do: Set Up

For the guided portion of this lesson we'll be working with only two files: `index.html` and `script.js`. We've already created these for you and included some starter code. Clone them down using the following Terminal command...

```bash
$ git clone https://github.com/ga-wdi-exercises/events-callbacks-practice.git
```

You should see this in `index.html`...

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
  </body>
  <script src="script.js"></script>
</html>
```

Now let's put a simple block of code in `script.js` to make sure it's properly linked to `index.html`

```js
// script.js

console.log( "The page's contents have finished loading!" );
```

> Is this code running syncronously or asynchronously?

## Setting Up An Event Listener (15 minutes / 0:25)

Now that we know a bit about events in JavaScript, let's wire up our code to be able to respond to those events. In order to run code in response to an event, we need to define an **Event Listener**. Below you'll find a simple event listener. 

```js
// script.js

var button = $("button");

function handleClickEvent(){
  console.log("I was clicked!");
}

// This is the event listener
button.on("click", handleClickEvent);
```
> What does it do?

Let's go through the above code examples line-by-line...

### Selecting The Element

We want our "click handler" -- what we're calling this event listener -- to trigger every time the button is clicked.
In order for this to happen, we need to select the button from the DOM and store a reference to it.

```js
var button = $("button");
```

<details>
<summary><strong>What exactly is being saved to `var button`?</strong></summary>

> In `$(selector)`, `selector` is a string interpreted as a [CSS-style selector](https://api.jquery.com/category/selectors/). All elements match by this selector are wrapped in a **jQuery object** letting us call more jQuery methods on the object. In the above example, the jQuery object contains the `<button>` element from our HTML.

> When there are multiple elements that match the provided selector, we can reference a specific one using a special jQuery method [`.eq(index)`](https://api.jquery.com/eq/) that returns JQuery object containing the element with that index in the collection. 

> What might be a better option than `$el.eq(idx)` (especially if we have control over the HTML)

> Do not use bracket notation  (`[idx]`) to access a single jQuery element out of a collection unless you want the underlying DOM element without the JQuery wrapper. Trying to call JQuery methods on a vanilla DOM element will result in an error.

</details>

<details>

<summary><strong>How do we select elements in jQuery vs. vanilla Javascript?</strong></summary>

> In vanilla javascript, we would use `.querySelector()`, however in jQuery we ***wrap it in ca$h*** using the `$()` function which has some added functionality than its vanilla js counterpart. If `.querySelector()` were a kitchen knife, `$()` would be a swiss army knife or a multi-tool.

</details>

### Defining The Behavior

When our button is clicked, we want some code to run that prints a message to the console. We are going to **encapsulate** that code in a function. This is the primary utility of a function - packaging up behavior and giving it a name so that it can be invoked at some later time.

```js
function handleClickEvent(){
  console.log("I was clicked!");
}
```

### Creating The Event Listener

Now for the start of the show: registering our function (encapsulated behavior) with the button to be run in the event of a "click". This is called adding an event listener.

```js
button.on("click", handleClickEvent);
```

<details>

<summary><strong>Vanilla Javascript implementation</strong></summary>

```js
var button = document.querySelector("button")
button.addEventListener("click", handleClickEvent)
```

</details>

#### `button`

We call the `.on` method of the JQuery object containing the DOM element(s) to which we are applying the listener. 

In this case, that JQuery object is `button` and the wrapped DOM element is the `<button>`. You can see in the REPL, the variable `button` evaluates to this JQuery object and `button[0]` or `button.get(0)` returns the contained DOM object.

The description of the underlying element is for the sake of understanding. You will not likely want to go back and forth between the two.

<details>

<summary><strong>Vanilla Javascript implementation</strong></summary>

```js
var vanillaButton = document.querySelector("button");
```

</details>

#### [`.on`](http://api.jquery.com/on/)

The jQuery method `.on(event, handler)` registers our event listener.

<details>

<summary><strong>Vanilla Javascript implementation</strong></summary>

```js
vanillaButton.addEventListener("click", handleClickEvent);
```

</details>


#### `"click"`

The first argument is where we indicate what type of event we are listening to. In this case, that is a "click". We could replace this with other events like `"submit"` or `"mouseover"`.

> An extensive list of Javascript events can be found [here](https://developer.mozilla.org/en-US/docs/Web/Events).

#### `handleClickEvent`

The second argument is our function defined above. This is what is known as a **callback**.

A callback is a function that is passed as an argument to another function.
The receiving function can then invoke the callback function whenever it would like.
We will explore this idea in more detail in a later lesson.

**A callback is a function that will be run at some later time.**

The invocation may happen immediately, after some period of time, or repeatedly. In the example above, `handleClickEvent` is our callback. The invocation happens whenever the button is clicked.

> If we are only using the callback for the single event listener, we may choose to define it as a function expression inline when registering the event (also known as an anonymous function -- since we are never calling it later by name, we do not need to give it a name). This might save a little typing but I personally prefer the clarity of an independently defined function.

### Callbacks: Calling vs. Referencing (5 minutes / 0:40)

When we call a function, we place parentheses after the function name -- this is invocation. The function is evaluated and returns a value based on the function definition. 

In the case of callbacks like `handleClickEvent`, however, there are no parens.

A function's name without parentheses is a reference to the function. The reference evaluates to the function itself. This ability to reference functions without calling them is what lets us pass functions around as values.

Try adding parentheses at the end of handleClickEvent

```js
button.on("click", handleClickEvent());
```

<details>

<summary><strong>Refresh the page. What happened? What was different? Why?</strong></summary>

> "I was clicked!" pops up immediately upon reload. When we include `()` we invoke `handleClickEvent` immediately, not after the user interacts with the page. Without the `()`, we're using the function expression as a reference.

</details>

### You Do: Practice (10 minutes / 0:35)

> 5 minutes exercise. 5 minutes review.

Visit this [repository](https://github.com/ga-wdi-exercises/event-listener-practice) and follow the instructions.

## Break (10 minutes / 0:50)

### You Do: [Color Scheme Switcher](https://github.com/ga-dc/color-scheme-switcher) (20 minutes / 1:10)

> 15 minutes exercise. 5 minutes review.

### `$(this)` (5 minutes / 1:15)

Let's switch back to our `events-callbacks-practice` code.

In JavaScript, we will see the keyword `this` quite a bit, especially when we get to object-oriented programming. It's closely related to the idea of **context**, the object on which a method was called. Much more on that tomorrow.

With that in the back of our minds, let's have a look at how we can use `this` or, in the case of jQuery, `$(this)`.

```js
var button = $("button")
var handleClickEvent = function(){
  console.log("I was clicked!")
}
button.on("click", handleClickEvent);
```

Insert the following lines anywhere in our click-handler...

```js
console.log($(this))
```

Refresh the page. What do you see when you click the button?

<details>

<summary><strong>With that in mind, how would you explain `this` in the context of an event listener?</strong></summary>

In the context of an event listener callback, `$(this)` always refers to the object on which the listener is registered.

</details>

<details>

  <summary><strong>What is the difference between `$(this)` and `this`? More importantly, how could you check that out?</strong></summary>

  `$(this)` returns a jQuery object. `this` returns a vanilla Javascript object.

</details>

### You Do: [`this` Practice](https://github.com/ga-wdi-exercises/events-this-practice) (15 minutes / 1:30)

> 10 minutes exercise. 5 minutes review.

### Bonus: [Implement `this` in Color Picker](https://github.com/ga-dc/color-scheme-switcher)

<details>

  <summary><strong>Solution</strong></summary>

  If you're curious, here's a short solution to the earlier Color Scheme Switcher exercise that makes use of `this`. Keep in mind, this is advanced! You are not expected to be able to write code like this yet.  

  ```js
  $("li").on("click", function () {
    $("body").attr("class", $(this).attr("class"));
  });
  ```

</details>

## Timing Functions (15 minutes / 1:45)

Let's look at timing functions -- that is, Javascript's way of making something happen after some period of time.

Keep in mind this [will not be totally precise](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout#Reasons_for_delays_longer_than_specified).

Replace the contents of your `script.js` with...

```js
function sayHello(){
  console.log("Hi there!")
}
setInterval(sayHello, 1000);
```

### Turn and Talk

Refresh the page. Spend two minutes observing and answering the following questions...

* What does `setInterval` do?
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
// Represent the start and stop buttons in Javascript
var start = $("#start");
var stop = $("#stop");

// Create a callback function that prints something to the console
var singAnnoyingSong = function(){
  console.log("I know a song that gets on everybody's nerves...");
}

// Create a variable that will store our timer
var songTimer;

// Create an event listener for the start button that will print something to the console every tenth of a second
start.on("click", function(){
  songTimer = setInterval(singAnnoyingSong, 100);
});

stop.on("click", function(){
  clearInterval(songTimer);
});
```

> We define `songTimer` outside of the final two event listeners because we need to be able to access it within both event listeners. This will make more sense when we discuss **scope** during tomorrow's class.

### Turn And Talk

Refresh the page. Observe and spend three minutes answering the following questions...

* What happens when you click the start button?
* What does `clearInterval` do?
* What happens when you click the "start" button a bunch of times in a row?

## Demo: Asynchronicity with Timers

Run the next bit of code and you can see asynchronous program execution.

```js
function anAsyncFunction(){
  console.log("hello")
  setTimeout(function(){
    console.log("this is happening in the middle")
  }, 3000)
  console.log("goodbye")
}

anAsyncFunction();
```

Wait, what? The goodbye came before the "this is happening in the middle"!

In the code we've written so far, JavaScript is interpreted and evaluated line by line. When one line is done, the next line is evaluated. This is called **synchronous** execution.

However, some operations in Javascript are **asynchronous**, meaning evaluation moves on to the next line and the environment handles execution of the asynchronous code when the synchronous code is done evaluating.

#### Why doesn't Javascript wait for these operations to complete before going to the next line of code?

While asynchronous code requires greater attention to detail and can be harder to reason about. It is enormously valuable.

Synchronous code is sometimes called **blocking** code. A long running task would cause our webpage to hang until the operation completes. The browser can't do anything while JavaScript is actively running. We've seen this when we've encountered infinite `while` loops. Asynchronicity is a way of preventing the browser from freezing.

This risk is greatest when Javascript is making requests to other webpages. There's no way of knowing how long the request will take to complete. It could be near-instant, but if the target server is having a bad day, it could take who-knows-how-long. You don't want the UX of your site to be at the mercy of some random computer somewhere else.

In this small app we made, anything we want to be sure happens **after** those 5 seconds of computing should go inside the callback of the `setTimeout`. This way, we can be certain that it will run only after the 5 seconds are up.

## You Do: [TimerJS](https://github.com/ga-wdi-exercises/timer_js) (40 minutes / 2:25)

> 30 minutes exercise . 10 minutes review.

-------

## Additional Topics

### Event Defaults

In `index.html`, replace your button with a link to Google...

```html
<body>
  <a href="http://google.com">Click me!</a>
</body>
```

Now, add an event listener to that link that brings up a `confirmation` prompt, asking the user if they want to go to Google...

```js
var link = $("a");
var handleClickEvent = function(evt){
  var input = confirm("You sure you want to go to Google?");
}
link.on("click", handleClickEvent);
```

> Aside: What do you notice about `confirm` regarding the conversation we just had about synchronicity?
 
The problem is we don't know how to stop them from going to Google! They go anyway, whether they hit "OK" or "Cancel".

Some events have a default action they perform. In the case of a click on `<a>`, that action is following an `href`. You can prevent that default action with an Event method called `preventDefault`.

```js
var link = $("a")
var handleClickEvent = function(e){
  e.preventDefault();
  confirm("You sure you want to go to Google?")
}
link.on("click", handleClickEvent);
```

> Event is a Javascript "class" from which all Javascript events are generated. We will explore JavaScript classes in a later lesson. If you're curious, however, try `console.log(e)` inside of the event listener.

We have prevented the default behavior of going to Google.

In order to make it so they that **do** go to Google on clicking OK, but **don't** on clicking 'Cancel', we can use the fact that when you click 'Cancel' on a `confirm`, it returns `false`...

```js
var link = $("a")
var handleClickEvent = function(e){
  if(confirm("You sure you want to go to Google?")){
    e.preventDefault();
  }
}
button.on("click", handleClickEvent);
```

### The Event Object

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

#### Explore The Event Object

With your partner, spend three minutes clicking the button and exploring what properties the event (or `evt`) object contains. Look for...

* A way to figure out what element was clicked on.
* A way to tell the position of the mouse when it clicked.
* One other piece of useful or interesting information.

## Key Events

Let's explore some other events. Add a text input field into `index.html`...

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

#### Explore Key Events

With a partner, add an event listener for the `keyup` event to the input. Explore the `event` object again. **Can you find a way to tell which key was pressed?**

#### Review

<details>

  <summary>Your code in `script.js` should look something like this...</summary>

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

  > A cross-browser way of telling which key is pressed is using the `keyCode` property. For `d`, `evt.keyCode` is `68`. For Shift, it's `16`.

</details>

#### Explore More Key Events

Find the keyCodes for...
* Enter
* Tab
* Delete

#### Explore Even More Input Events

There are several other events that come up with the `input` tag. See if you can figure out the difference between...

* `keyup`
* `keydown`
* `keypress`
* `change`
* `focus`
* `blur`

> If you want to test out more Javascript events, an extensive list can be found [here](https://developer.mozilla.org/en-US/docs/Web/Events).

-------

## Sample Quiz Questions

1. What is the difference between synchronous and asynchronous program execution?
2. Define a function that takes a function as an argument and invokes the argument when the function is called.
3. What arguments does `setInterval` take?
4. What is the difference between `setInterval` and `setTimeout`?

## Homework: [Pixart](https://github.com/ga-wdi-exercises/pixart_js)

## Screencasts

* [Robin's Screencast](https://youtu.be/S4Xvo_m6P04)
* [Andy's Screencast (Part I)](https://www.youtube.com/watch?v=xogI6prB-PI)
* [Andy's Screencast (Part II)](https://www.youtube.com/watch?v=Srd2Tx1Z7v8)
