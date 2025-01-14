
### Global Context

### Hoisting

Function declarations are scanned and made available (hoisted).
Variable declaration are scanned and made undefined. 

### Temporal Dead Zone

[[Temporal Dead Zone]]

### Execution Context

## Call Stack

The call stack is a mechanism that the JavaScript interpreter uses to keep track of function execution within a program. In JavaScript, functions are executed in the order they are called. The call stack follows the Last In, First Out (LIFO) principle, meaning that the last function pushed onto the stack is the first one to be executed.

According to the ECMAScript specification, the call stack is defined as part of the execution context. Whenever a function is called, a new execution context is created and placed at the top of the stack. Once the function completes, its execution context is removed from the stack, and control returns to the previous context. This helps manage synchronous code execution, as each function call must complete before the next one can begin.

## Primitive Types

According to the ECMAScript specification, JavaScript has six primitive data types: string, number, bigint, boolean, undefined, and symbol. These types are immutable, meaning their values cannot be altered. There is also a special primitive type called null, which represents the intentional absence of any object value.

Primitive values are directly assigned to a variable, and when you manipulate a primitive type, you're working directly on the value. Unlike objects, primitives do not have properties or methods, but JavaScript automatically wraps primitive values with object counterparts when necessary (e.g., when calling methods on strings).

## Value Types and Reference Types

According to the ECMAScript specification, value types are stored directly in the location that the variable accesses. These include types like number, string, boolean, undefined, bigint, symbol, and null. When you assign a value type to a variable, the value itself is stored.


## Implicit, Explicit, Nominal, Structuring and Duck Typing

The ECMAScript specification defines JavaScript as a dynamically typed language, meaning that types are associated with values rather than variables, and type checking occurs at runtime. There are various ways JavaScript manages types:

Implicit Typing (or Type Coercion): This occurs when JavaScript automatically converts one data type to another when required. For instance, JavaScript might convert a string to a number during an arithmetic operation. While this can simplify some code, it can also lead to unexpected results if not handled carefully.

Explicit Typing: Unlike implicit typing, explicit typing involves manually converting a value from one type to another using functions like Number(), String(), or Boolean().

Nominal Typing: JavaScript doesn't natively support nominal typing, where types are explicitly declared and checked. However, TypeScript, a superset of JavaScript, brings this feature to help catch type errors during development.

Structuring Typing: In this type system, types are based on the structure or properties of the data. JavaScript is a structurally typed language where objects are compatible if they share the same structure (i.e., the same set of properties and methods).

Duck Typing: This is a concept where an object's suitability is determined by the presence of certain properties and methods, rather than by the actual type of the object. JavaScript relies heavily on duck typing, where behavior is inferred from an object's properties rather than its declared type.

[Duck typing](https://en.wikipedia.org/wiki/Duck_typing) is a concept in programming that is often used in dynamic programming languages such as JavaScript, Python, and Ruby. It is a method of determining the type of an object based on its behavior or the methods and properties it possesses, rather than its explicit type or class.

The term “duck typing” comes from the saying, “If it looks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck.” In programming, this means that if an object behaves like a certain type, then it can be treated as that type, regardless of its actual class or type.

For example, in JavaScript, you can use duck typing to check whether an object is iterable by checking whether it has a `Symbol.iterator` method. This method is used to define the iteration behavior of an object, and if an object has this method, it can be treated as iterable, even if it doesn’t explicitly implement the Iterable interface.

https://byby.dev/js-duck-typing


## == vs === vs typeof

According to the ECMAScript specification, JavaScript includes both strict (===) and loose (==) equality operators, which behave differently when comparing values. Here's a breakdown:

== (Loose Equality): This operator performs type coercion before comparing two values. If the values are of different types, JavaScript will attempt to convert one or both values to a common type before comparison, which can lead to unexpected results.

=== (Strict Equality): This operator compares both the value and the type without any type coercion. If the two values are not of the same type, the comparison will return false.

typeof Operator: The typeof operator is used to check the data type of a variable. While it's generally reliable, there are certain quirks, like how typeof null returns "object" instead of "null", due to a long-standing behavior in JavaScript's implementation.


## Function Scope, Block Scope and Lexical Scope


The ECMAScript specification outlines three key types of scope:

Function Scope: Variables declared within a function using var are only accessible within that function. This scope isolates variables from being accessed outside of the function where they are declared.

Block Scope: Introduced with ES6, variables declared with let and const are block-scoped. This means they are only accessible within the specific block {} in which they are defined, such as inside loops or conditionals.

Lexical Scope: Refers to how variable access is determined based on the physical location of the variables in the code. Functions are lexically scoped, meaning that they can access variables from their parent scope.

## Expression vs Statement

According to the ECMAScript specification, expressions produce a value, and statements are instructions to perform an action, such as variable assignment or control flow. Function declarations are hoisted and can be called before they are defined in the code, while function expressions are not hoisted and must be defined before being invoked.

## IIFE, Modules and Namespaces

With the introduction of ES6 modules, the role of IIFEs in scope isolation has diminished but they still remain relevant.

**_Immediately-invoked Function Expression._** Or more dearly known as **_IIFE_** and pronounced as **_“iffy.”_**

Before we can understand what an IIFE is and why we need one, we need to review a few fundamental concepts around JavaScript functions quickly.

## The natural function definition

Developers new to JavaScript are naturally comfortable with the following syntax when dealing with functions.

```javascript
function sayHi() {
	alert("Hello, World!");
}

sayHi(); // shows "Hello, World!" as alert in browser.|
```

1. Lines 1–3 define a function named **_sayHi._**
2. On line 5 we call it with the usual “**_()”_** syntax to invoke the function.

This way of creating a function is called “_a function definition”_ or _“a function declaration”_ or “_a function statement”_. Usually, developers new to JavaScript have no trouble using this syntax as it closely resembles functions/methods in other popular programming languages.

> These function definitions always start with the **function** keyword and are always followed by a name for the function. You can’t omit the name as it’s invalid syntax.

## Function expressions

This is when things start to get more interesting in JavaScript. Let’s see how a function expression looks like.

```javascript
var msg = "Hello, World!";
var sayHi = function() {
    alert(msg);
};

sayHi(); // shows "Hello, World!" as alert in browser.
```

This seemingly simple example can help you take your JavaScript skills to the next level.

1. Line 1 declares **_msg_** variable and assigns a **_string_** value to it.
2. Lines 2–4 declare **_sayHi_** variable and assign a value to it that’s of **_function_** type.
3. Line 6 calls this **_sayHi_** function.

Line 1 is trivial to understand. But when developers see lines 2–4 for the first time, it usually defies their expectations if they are coming from other programming languages like Java.

> Basically, in lines 2–4 we assigned a value that’s of **function** type to a variable named **sayHi**.
> 
> In the above example, the function on the right-hand side of the assignment operator is often called a **“function expression.”** They are everywhere in JavaScript. Most callbacks you might have written are often function expressions.

You might have used these function expressions without understanding the underpinnings. But mastering them will give you some secret JavaScript superpowers.

> So the important concept to remember here is that functions are almost like any other values in JavaScript. They can be on the right-hand side of an assignment operator, or they can be passed as arguments to other functions.

## Anonymous function expressions

Well, you already know what they are. The above example was an anonymous function expression. They are anonymous because they don’t have a name following the **_function_** keyword.

## Named function expressions

Function expressions can have names. The most boring and universally explained usage of these named function expressions is with recursion. Don’t worry much about these now as you can master IIFE without understanding named function expressions.

```javascript
var fibo = function fibonacci() {
    // you can use "fibonacci()" here as this funciton expression has a name.
};

// fibonacci() here fails, but fibo() works.
```

So the difference here is that the function expression has a name “**_fibonacci”_** that can be used inside that function expression to call itself recursively. (There is more to it like the function name shows up in stack-traces and etc, but let’s not worry about them in this tutorial.)

## Enough! Show me an IIFE or I am leaving now!

Thanks for being patient and patience is the most important skill that’s needed to master JavaScript!

Now that you have learned function definitions and function expressions, let’s dive right into the secret world of IIFEs. They come in a few stylistic variations. Let’s first see a variation that’s really, really easy to understand.

```javascript
!function() {
    alert("Hello from IIFE!");
}();
// Shows the alert "Hello from IIFE!"
```

That’s my friends, is our dear IIFE in action! When you copy this code and try in a browser’s console, you will see the alert from code on line 2. And that’s pretty much it. No one can ever get this alert to show up again.

> That’s a function that died immediately after it came to life.

Now let’s understand that not so intuitive syntax: I know you spotted that **_“!”_** on line 1; If you did not, no worries, you would have spotted it now!

1. As we saw before, a function statement always starts with the **_function_** keyword. Whenever JavaScript sees **_function_** keyword as the first word in a valid statement, it expects that a function definition is going to take place. So to stop this from happening, we are prefixing **_“!”_** in-front of the **_function_** keyword on line 1. This basically enforces JavaScript to treat whatever that’s coming after **_“!”_** as an expression.
2. But the most interesting stuff happens on line 3 where we execute that function expression immediately.

> So we have a function expression that’s immediately invoked after it’s created. And that’s, my friends, is called an IIFE irrespective of the stylistic variation used to achieve this effect.

The above stylistic variation can be used by replacing **_“!”_** with **_“+”, “-”,_** or even **_“~”_** as well. Basically any unary operator can be used.

Now, give it a try in the console! And play around with the IIFE to your joy!

> All that the first character, “**!”**, is doing here is to make that function into an expression instead of a function statement/definition. And then we execute that function immediately.

Another quick variation on this is shown below:

```javascript
void function() {
    alert("Hello from IIFE!");
}();
```

Again **_void_** is basically forcing the function to be treated as an expression.

All the above patterns are useful when we are not interested in the return value from the IIFE.

But then what if you wanted a return value from the IIFE and you want to use that return value elsewhere? Read on to know the answer!

## Classical IIFE style

The IIFE pattern we have seen above is easy to understand. So I started with that style first instead of the other more traditional and widely used style.

> As we saw in the above IIFE examples, the key to IIFE pattern is taking a function and turning it into an expression and executing it immediately.

First, let’s see yet another way to make a function expression then!

```javascript
(function() {
    alert("I am not an IIFE yet!");
});
```

In the above code, a function expression is wrapped in parentheses in lines 1–3. It’s not yet an IIFE as that function expression is never ever executed. Now to convert that code into an IIFE, we have following two stylistic variations:

```javascript
// Variation 1
(function() {
    alert("I am an IIFE!");
}());

// Variation 2
(function() {
    alert("I am an IIFE, too!");
})();
```

Now we have got two IIFEs in action. It might be really tough to notice the difference between Variation 1 and Variation 2. So let me explain that.

1. In Variation 1, on line 4, parentheses **_()_** for invoking the function expression is contained inside the outer parentheses. Again outer parentheses are needed to make a function expression out of that function.
2. In Variation 2, on line 9, parentheses **_()_** for invoking the function expression is outside the wrapping parentheses for the function expression.

Both variations are used widely. I personally prefer Variation 1. If we get into the nitty-gritty, both variations differ slightly on how they work. But for all practical purposes and keeping this already long tutorial short, I am going to say you can use either one of them to your liking. _(I will link to a future article that I plan to write on_ **_()_** _operator and_ **_comma_** _operator that clarifies nuances between these two styles.)_

### Modules

https://ponyfoo.com/articles/es6-modules-in-depth#strict-mode

## Message Queue and Event Loop

The Event Loop is a critical part of JavaScript’s concurrency model, ensuring non-blocking behavior by processing tasks in an asynchronous manner. Understanding how it interacts with the Message Queue and Microtasks is key to mastering JavaScript behavior.

https://hackernoon.com/understanding-js-the-event-loop-959beae3ac40

### What’s the Event Loop?

You’ve probably heard that JavaScript is a single-threaded language. You may have even heard the terms Call Stack and Event Queue. Most people know that the Event Loop is what allows JavaScript to use callbacks and promises, but there’s a lot more to it. Without going into too much details we’ll have a high level view of how JavaScript code is actually executed.

### The Call Stack

JavaScript has a single call stack in which it keeps track of what function we’re currently executing and what function is to be executed after that. But first — what’s a stack? A stack is an array-like data structure but with some limitations — you can only add items to the back and only remove the last item. Another example is a pile of plates — you put them on top of each other and at any time you can only remove the top one.

When you’re about to execute a function it is added on the call stack. Then if that function calls another function — the other function will be on top of the first one in the call stack. When you get an error in the console you get a long message that shows you the path of execution — this is what the stack looked in that exact moment. But what if we make a request or put a timeout on something? In theory that should freeze the entire browser until it is executed so the call stack can continue? In practice however, you know that this doesn’t happen — because of the Event Table and Event Queue.

### The Event Table & Event Queue

Every time you call a setTimeout function or you do some async operation — it is added to the Event Table. This is a data structure which knows that a certain function should be triggered after a certain event. Once that event occurs (timeout, click, mouse move) it sends a notice. Bear in mind that the Event Table does not execute functions and does not add them to the call stack on it’s own. It’s sole purpose is to keep track of events and send them to the Event Queue.

The Event Queue is a data structure similar to the stack — again you add items to the back but can only remove them from the front. It kind of stores the correct order in which the functions should be executed. It receives the function calls from the Event Table, but it needs to somehow send them to the Call Stack? This is where the Event Loop comes in.

### The Event Loop

We’ve finally reached the infamous Event Loop. This is a constantly running process that checks if the call stack is empty. Imagine it like a clock and every time it _ticks_ it looks at the Call Stack and if it is empty it looks into the Event Queue. If there is something in the event queue that is waiting it is moved to the call stack. If not, then nothing happens.

Here are a couple of interesting cases. In what order do you think the following code will run?

setTimeout(() => console.log('first'), 0)console.log('second')

Some people think that because set timeout is called with 0 (zero) it should run immediately. In fact in this specific example you will see “second” printed out before “first”. JavaScript sees the setTimeout and says “Well, I should add this to my Event Table and continue executing”. It will then go through the Event Table, Event Queue and wait for the Event Loop to _tick_ in order to run.

https://www.youtube.com/watch?v=eiC58R16hb8

## setTimeout, setInterval

https://javascript.info/settimeout-setinterval

## JavaScript Engines

JavaScript engines are inbuilt in all the modern browsers today. When the JavaScript file is loaded in the browser, JavaScript engine will execute each line of the file from top to bottom (to simplify the explanation we are avoiding hoisting in JS). JavaScript engine will parse the code line by line, convert it into machine code and then execute it. Some of the well-known JS engines are listed below:

![](https://miro.medium.com/v2/resize:fit:886/1*_tZOWEoRIvRZV75ak-FAdg.png)

Browser engines

V8 engine: V8 is a powerful open source Javascript engine provided by Google. So what actually is a Javascript Engine? It is a program that converts Javascript code into lower level or machine code that microprocessors can understand.

There are different JavaScript engines including [Rhino](https://en.wikipedia.org/wiki/Rhino_(JavaScript_engine)), [JavaScriptCore](https://en.wikipedia.org/wiki/WebKit#JavaScriptCore), and [SpiderMonkey](https://en.wikipedia.org/wiki/SpiderMonkey_(JavaScript_engine)). These engines follow the ECMAScript Standards. ECMAScript defines the standard for the scripting language. JavaScript is based on ECMAScript standards. These standards define how the language should work and what features it should have. You can learn more about ECMAScript [here](https://www.ecma-international.org/publications/standards/Ecma-262.htm).

![Image](https://cdn-media-1.freecodecamp.org/images/1*gZq22sBm1y3eq1NfhEaXeg.png)_Source: Google_

The Chrome V8 engine :

- The V8 engine is written in C++ and used in Chrome and Nodejs.
- It implements ECMAScript as specified in ECMA-262.
- The V8 engine can run standalone we can embed it with our own C++ program.

https://www.youtube.com/watch?v=oc6faXVc54E

## Document Object Model (DOM)

The **Document Object Model** (**DOM**) connects web pages to scripts or programming languages by representing the structure of a document—such as the HTML representing a web page—in memory. Usually it refers to JavaScript, even though modeling HTML, SVG, or XML documents as objects are not part of the core JavaScript language.

The DOM represents a document with a logical tree. Each branch of the tree ends in a node, and each node contains objects. DOM methods allow programmatic access to the tree. With them, you can change the document's structure, style, or content.

Nodes can also have event handlers attached to them. Once an event is triggered, the event handlers get executed.

To learn more about what the DOM is and how it represents documents, see our article [Introduction to the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction).

## Classes

https://www.digitalocean.com/community/tutorials/better-javascript-with-es6-pt-ii-a-deep-dive-into-classes#base-classes-declarations-and-expressions

## Factory Functions

https://www.youtube.com/watch?v=jpegXpQpb3o
## apply , bind and call

# Function.prototype.apply()

The **`apply()`** method of [`Function`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function) instances calls this function with a given `this` value, and `arguments` provided as an array (or an [array-like object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Indexed_collections#working_with_array-like_objects)).

```js
const numbers = [5, 6, 2, 3, 7];
const max = Math.max.apply(null, numbers);
console.log(max);
// Expected output: 7

const min = Math.min.apply(null, numbers);
console.log(min);
// Expected output: 2
```
## [Syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#syntax)

```js
apply(thisArg)
apply(thisArg, argsArray)
```
### [Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#parameters)

[`thisArg`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#thisarg)

The value of `this` provided for the call to `func`. If the function is not in [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode), [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null) and [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) will be replaced with the global object, and primitive values will be converted to objects.

[`argsArray` Optional](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#argsarray)

An array-like object, specifying the arguments with which `func` should be called, or [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null) or [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) if no arguments should be provided to the function.

### [Return value](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#return_value)

The result of calling the function with the specified `this` value and arguments.

## [Description](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#description)

**Note:** This function is almost identical to [`call()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call), except that the function arguments are passed to `call()` individually as a list, while for `apply()` they are combined in one object, typically an array — for example, `func.call(this, "eat", "bananas")` vs. `func.apply(this, ["eat", "bananas"])`.


# Function.prototype.call()

The **`call()`** method of [`Function`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function) instances calls this function with a given `this` value and arguments provided individually.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

console.log(new Food('cheese', 5).name);
// Expected output: "cheese"
```

## Syntax

```js
call(thisArg)
call(thisArg, arg1)
call(thisArg, arg1, arg2)
call(thisArg, arg1, arg2, /* …, */ argN)
```


### [Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call#parameters)

[`thisArg`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call#thisarg)

The value to use as `this` when calling `func`. If the function is not in [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode), [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null) and [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) will be replaced with the global object, and primitive values will be converted to objects.

[`arg1`, …, `argN` Optional](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call#arg1)

Arguments for the function.
### [Return value](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call#return_value)

The result of calling the function with the specified `this` value and arguments.

# Function.prototype.bind()

The **`bind()`** method of [`Function`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function) instances creates a new function that, when called, calls this function with its `this` keyword set to the provided value, and a given sequence of arguments preceding any provided when the new function is called.

```js

const module = {
  x: 42,
  getX: function () {
    return this.x;
  },
};

const unboundGetX = module.getX;
console.log(unboundGetX()); // The function gets invoked at the global scope
// Expected output: undefined

const boundGetX = unboundGetX.bind(module);
console.log(boundGetX());
// Expected output: 42
```

## Syntax

```js
bind(thisArg)
bind(thisArg, arg1)
bind(thisArg, arg1, arg2)
bind(thisArg, arg1, arg2, /* …, */ argN)
```

### [Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#parameters)

[`thisArg`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#thisarg)

The value to be passed as the `this` parameter to the target function `func` when the bound function is called. If the function is not in [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode), [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null) and [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) will be replaced with the global object, and primitive values will be converted to objects. The value is ignored if the bound function is constructed using the [`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) operator.

[`arg1`, …, `argN` Optional](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#arg1)

Arguments to prepend to arguments provided to the bound function when invoking `func`.

### [Return value](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#return_value)

A copy of the given function with the specified `this` value, and initial arguments (if provided).

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply

https://www.youtube.com/watch?v=uBdH0iB1VDM


### new Operator

https://codeburst.io/javascript-for-beginners-the-new-operator-cee35beb669e

# The Four Rules.

The simplest way to understand the `new` operator is to understand what it does. When you use `new`, four things happen:

1. It creates a new, empty object.
2. It binds `this` to our newly created object.
3. It adds a property onto our newly created object called “__proto__” which points to the constructor function’s prototype object.
4. It adds a `return this` to the end of the function, so that the object that is created is returned from the function.

Create a constructor function called `Student`. This function will take two parameters, `name`, and `age`. It will then set these properties on the value of `this`.

```js
function Student(name, age) {  
  this.name = name;  
  this.age = age;  
}
```

Perfect. Now lets invoke our constructor function with the `new` operator. We’re going to pass in two arguments: `'John'`, and `26`.

```js
var first = new Student('John', 26);
```

So, what happens when we run the above code?

1. A new object is created — the `first` object.
2. `this` is bound to our `first` object. So any references to `this` will point to `first`.
3. Our __proto__ is added. `first.__proto__` will now point to `Student.prototype`.
4. After everything is done, our brand new `first` object is returned to our new `first` variable.

We can now run a few simple `console.log` statements to test if it worked:

```js
console.log(first.name);  
// Johnconsole.log(first.age);  
// 26
```

Awesome. Let’s dive more into the `__proto__` portion of the new keyword.
# Prototypes

==**_Every JavaScript object has a prototype. All objects in JavaScript inherit their methods and properties from their prototypes._**==

Lets check out an example. Open up your Chrome Developer Console (Windows: Ctrl + Shift + J)(Mac: Cmd + Option + J) and type the Student` function from earlier in this article:

```js
function Student(name, age) {  
  this.name = name;  
  this.age = age;  
}
```

To prove that every object has a prototype, lets now type:

```js
Student.prototype;  
// Object {...}
```

Cool, an object is returned. Now lets try creating a new student:

```js
var second = new Student('Jeff', 50);
```

We’ve used our `Student` constructor function to create our second student named Jeff. And, since we used the `new` operator, the `__proto__` property should have also been added to our `second` object. It should point to the parent constructor. Lets try seeing if they’re equal:

```js
second.__proto__ === Student.prototype;  
// true
```

Finally, our `Student.prototype.constructor` should point to our `Student`constructor function:

```js
Student.prototype.constructor;//  function Student(name, age) {  
//    this.name = name;  
//    this.age = age;  
//  }
```

This is getting complicated very quickly. Lets see if a crappy _paint_ image doesn’t help you visualize what is going on:

![](https://miro.medium.com/v2/resize:fit:1188/1*RIY6LBqQAbXPzMXMwH5d-Q.png)

It’s beautiful, isn’t it?

As you can see above, our `Student` constructor function (as well as all other constructor functions) have a property called `.prototype`. This prototype has an object on it called `.constructor` which points back to the constructor function. It’s a nice little loop. Then, when we use the `new` operator to create a new object, each object has `.__proto__` property which links the new object back to the `Student.prototype`.

So why is this so important?

> **It’s important because of inheritance. The prototype object is shared among all objects created with that constructor function. This means we can add functions and properties to the prototype that all of our objects can use**.

In our above examples, we only created two `Student` objects, but what if instead of two students, we have 20,000? All of a sudden, we’re saving a ton of processing power by putting shared functions on the prototype instead of in each of the student objects.

Lets look at an example to drive this idea home. In your console add the following line:

```js
Student.prototype.sayInfo = function(){  
  console.log(this.name + ' is ' + this.age + ' years old');  
}
```

Again, what we’re doing here is adding a function to the Student prototype — Any student we create or have created now has access to this brand new `.sayInfo` function! Let’s test it out:

```js
second.sayInfo();  
// Jeff is 50 years old
```

Add a new student and try it again:

```js
var third = new Student('Tracy', 15);// Now if we log third out, we see the object only has two  
// properties, age and name. Yet, we still have access to the   
// sayInfo function:third;  
Student {name: "Tracy", age: 15}
third.sayInfo();  
// Tracy is 15 years old
```

It works! And it works because of _inheritance_. With JavaScript objects, an object will first look to see if it has the property we are calling. If it doesn’t, it then moves upwards, to it’s prototype and says ‘_Hey, do you have this property?_’. This same pattern continues all the way up until we either find that property, or we reach the end of the prototype chain at the global object.

Inheritance is the same reason you’ve been able to use functions like `.toString()` in the past! Think about it, you’ve never written a `toString()`method, yet you’ve been able to use it just fine. That’s because the method, as well as other built in JS methods are on the `Object prototype`. Every object we create ultimately delegates to the `Object prototype`. And sure, we could over write these methods with something like this:

```js
var name = {  
  toString: function(){  
    console.log('Not a good idea');  
  }  
};
name.toString();  
// Not a good idea
```

Our object first checks to see if it has the method before moving to the prototype. Since we do have the method, it is run and there is no inheritance needed. But that’s not a good idea. Leave global methods as they are and name your functions something else.

# Object.assign() 

The **`Object.assign()`** static method copies all [enumerable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/propertyIsEnumerable) [own properties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwn) from one or more _source objects_ to a _target object_. It returns the modified target object.

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// Expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget === target);
// Expected output: true
```

```js
Object.assign(target)
Object.assign(target, source1)
Object.assign(target, source1, source2)
Object.assign(target, source1, source2, /* …, */ sourceN)
```

# Object.create()

The **`Object.create()`** static method creates a new object, using an existing object as the prototype of the newly created object.

```js
const person = {
  isHuman: false,
  printIntroduction: function () {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  },
};

person.printIntroduction();
// "My name is undefined. Am I human? false"

const me = Object.create(person);

me.name = 'Matthew'; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // Inherited properties can be overwritten

me.printIntroduction();
// Expected output: "My name is Matthew. Am I human? true"
```

### [Classical inheritance with Object.create()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create#classical_inheritance_with_object.create)

Below is an example of how to use `Object.create()` to achieve classical inheritance. This is for a single inheritance, which is all that JavaScript supports.

```js
// Shape - superclass
function Shape() {
  this.x = 0;
  this.y = 0;
}

// superclass method
Shape.prototype.move = function (x, y) {
  this.x += x;
  this.y += y;
  console.info("Shape moved.");
};

// Rectangle - subclass
function Rectangle() {
  Shape.call(this); // call super constructor.
}

// subclass extends superclass
Rectangle.prototype = Object.create(Shape.prototype, {
  // If you don't set Rectangle.prototype.constructor to Rectangle,
  // it will take the prototype.constructor of Shape (parent).
  // To avoid that, we set the prototype.constructor to Rectangle (child).
  constructor: {
    value: Rectangle,
    enumerable: false,
    writable: true,
    configurable: true,
  },
});

const rect = new Rectangle();

console.log("Is rect an instance of Rectangle?", rect instanceof Rectangle); // true
console.log("Is rect an instance of Shape?", rect instanceof Shape); // true
rect.move(1, 1); // Logs 'Shape moved.'
```

### References

https://github.com/leonardomso/33-js-concepts?tab=readme-ov-file#1-call-stack

