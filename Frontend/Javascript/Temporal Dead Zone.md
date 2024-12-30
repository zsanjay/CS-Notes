
Temporal Dead Zone and Hoisting are two essential terms in JavaScript. But understanding how they work can easily confuse you if you don't approach them properly.

But don't fret! This article is here to help you get a good grasp of the two terms.

So relax, grab your favorite cup of coffee, and let’s get started with TDZ.

## What Exactly Is a Temporal Dead Zone in JavaScript?

A **temporal dead zone (TDZ)** is the area of a block where a variable is inaccessible until the moment the computer completely initializes it with a value.

- A [block](https://www.codesweetly.com/code-block/) is a pair of braces (`{...}`) used to group multiple statements.
- [Initialization](https://www.codesweetly.com/declaration-initialization-invocation-in-programming/) occurs when you assign an initial value to a variable.

Suppose you attempt to access a variable before its complete initialization. In such a case, JavaScript will throw a `ReferenceError`.

So, to prevent JavaScript from throwing such an error, you’ve got to remember to access your variables from outside the temporal dead zone.

But where exactly does the TDZ begin and end? Let’s find out below.

## Where Exactly Is the Scope of a Temporal Dead Zone?

A block’s temporal dead zone starts at the beginning of the block’s local scope. It ends when the computer fully initializes your variable with a value.

**Here’s an example:**

```js
{
  // bestFood’s TDZ starts here (at the beginning of this block’s local scope)
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  console.log(bestFood); // returns ReferenceError because bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  let bestFood = "Vegetable Fried Rice"; // bestFood’s TDZ ends here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
}
```

[**Try it on StackBlitz**](https://stackblitz.com/edit/js-sjp8hk?file=index.js)

In the snippet above, the block’s TDZ starts from the opening curly bracket (`{`) and ends once the computer initializes `bestFood` with the string value `"Vegetable Fried Rice"`.

When you run the snippet, you will see that the `console.log()` statement will return a `ReferenceError`.

JavaScript will return a `ReferenceError` because we used the `console.log()` code to access `bestFood` before its complete initialization. In other words, we invoked `bestFood` within the temporal dead zone.

However, here is how you can access `bestFood` successfully after its complete initialization:

```js
{
  // TDZ starts here (at the beginning of this block’s local scope)
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  let bestFood = "Vegetable Fried Rice"; // bestFood’s TDZ ends here
  console.log(bestFood); // returns "Vegetable Fried Rice" because bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
}
```

[**Try it on StackBlitz**](https://stackblitz.com/edit/js-utknki?file=index.js)

Now, consider this example:

```js
{
  // TDZ starts here (at the beginning of this block’s local scope)
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  let bestFood; // bestFood’s TDZ ends here
  console.log(bestFood); // returns undefined because bestFood’s TDZ does not exist here
  bestFood = "Vegetable Fried Rice"; // bestFood’s TDZ does not exist here
  console.log(bestFood); // returns "Vegetable Fried Rice" because bestFood’s TDZ does not exist here
}
```

[**Try it on StackBlitz**](https://stackblitz.com/edit/js-uyxf4y?file=index.js)

You can see that the first `console.log` code in the snippet above returned `undefined`.

JavaScript returned `undefined` because we did not assign `bestFood` a value before using ([invoking](https://www.codesweetly.com/declaration-initialization-invocation-in-programming/#what-does-invocation-mean-in-programming)) it. As such, JavaScript defaulted its value to `undefined`.

Keep in mind that you must specify a value for a `const` variable while [declaring](https://www.codesweetly.com/declaration-initialization-invocation-in-programming/#what-exactly-does-declaration-mean) it. Apart from this exception, all other temporal dead zone principles of `let` variables apply also to `const`. However, `var` works differently.

## How Does Var’s TDZ Differ from Let and Const Variables?

The main difference between the temporal dead zone of a `var`, `let`, and `const` variable is when their TDZ ends.

For instance, consider this code:

```js
{
  // bestFood’s TDZ starts and ends here
  console.log(bestFood); // returns undefined because bestFood’s TDZ does not exist here
  var bestFood = "Vegetable Fried Rice"; // bestFood’s TDZ does not exist here
  console.log(bestFood); // returns "Vegetable Fried Rice" because bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
}
```

[**Try it on StackBlitz**](https://stackblitz.com/edit/js-gcn7j5?file=index.js)

When you run the snippet above, you will see that the first `console.log`statement will return `undefined`.

The `console.log` statement successfully returned a value (`undefined`) because JavaScript automatically assigns `undefined` to a hoisted `var`variable.

In other words, when the computer hoists a `var` variable, it automatically initializes the variable with the value `undefined`.

In contrast, JavaScript does not initialize a `let` (or `const`) variable with any value whenever it hoists the variable. Instead, the variable remains dead and inaccessible.

Therefore, a `let` (or `const`) variable’s TDZ ends when JavaScript fully initializes it with the value specified during its declaration.

However, a `var` variable’s TDZ ends immediately after its hoisting—not when the variable gets fully initialized with the value specified during its declaration.

But what exactly does “hoisting” mean? Let’s find out below.

## What Exactly Does Hoisting Mean in JavaScript?

**Hoisting** refers to JavaScript giving higher precedence to the declaration of variables, classes, and functions during a program’s execution.

Hoisting makes the computer process declarations before any other code.

**Note:** Hoisting does not mean JavaScript rearranges or moves code above one another.

Hoisting simply gives higher specificity to JavaScript declarations. Thus, it makes the computer read and process declarations first before analyzing any other code in a program.

For instance, consider this snippet:

```js
{
  // Declare a variable:
  let bestFood = "Fish and Chips";

  // Declare another variable:
  let myBestMeal = function () {
    console.log(bestFood);
    let bestFood = "Vegetable Fried Rice";
  };

  // Invoke myBestMeal function:
  myBestMeal();
}

// The code above will return:
"Uncaught ReferenceError: Cannot access 'bestFood' before initialization"
```

[**Try it on StackBlitz**](https://stackblitz.com/edit/js-ymmkih?file=index.js)

The snippet above returned a `ReferenceError` because of the order of precedence by which the computer executed each code.

In other words, the program’s [declarations](https://www.codesweetly.com/declaration-initialization-invocation-in-programming#what-exactly-does-declaration-mean) got higher precedence over [initializations](https://www.codesweetly.com/declaration-initialization-invocation-in-programming#what-does-initialization-mean), [invocations](https://www.codesweetly.com/declaration-initialization-invocation-in-programming#what-does-invocation-mean-in-programming), and other code.

Let’s go through a step-by-step tour of how JavaScript executed the snippet above.

## How JavaScript Hoisting Works Step-by-Step

Below is a walk-through of how JavaScript executed the previous snippet.

### 1. JavaScript parsed the first `bestFood` declaration

```js
let bestFood // This is the first bestFood declaration in the program
```

The first `bestFood` variable declaration is the first code the computer analyzed.

Note that after the computer read the `bestFood` variable declaration, JavaScript automatically kept the variable in a _temporal dead zone_ until it got fully initialized.

Therefore, any attempt to access `bestFood` before its complete initialization would return a `ReferenceError`.

### 2. The computer parsed `myBestMeal` variable declaration

```js
let myBestMeal
```

The `myBestMeal` variable declaration was the second code JavaScript analyzed.

Immediately after the computer read the `myBestMeal` variable declaration, JavaScript automatically kept the variable in a temporal dead zone until it got fully initialized.

Therefore, any attempt to access `myBestMeal` before its complete initialization would return a `ReferenceError`.

### 3. The computer initialized the `bestFood` variable

```js
bestFood = "Fish and Chips";
```

The computer’s third step was to initialize `bestFood` with the `“Fish and Chips”` string value.

Therefore, invoking `bestFood` at this point would return `“Fish and Chips”`.

### 4. JavaScript initialized `myBestMeal` variable

```js
myBestMeal = function() {
  console.log(bestFood);
  let bestFood = "Vegetable Fried Rice";
};
```

Fourthly, JavaScript initialized `myBestMeal` with the specified function. So, if you had invoked `myBestMeal` at this point, the function would have returned.

### 5. The computer invoked `myBestMeal`’s function

```js
myBestMeal();
```

The invocation of `myBestMeal`’s function was the computer’s fifth action.

After the invocation, the computer processed each code in the function’s block. However, the declarations had higher precedence over other code.

### 6. JavaScript parsed the function’s `bestFood`declaration

```js
let bestFood // This is the second bestFood declaration in the program
```

JavaScript’s sixth task was to analyze the function’s `bestFood` variable declaration.

After the analysis, JavaScript automatically kept the variable in a temporal dead zone—until its complete initialization.

Therefore, any attempt to access `bestFood` before its complete initialization would return a `ReferenceError`.

### 7. The computer parsed the function’s `console.log`statement

```js
console.log(bestFood);
```

Finally, the computer read the `console.log` statement—which instructed the system to log `bestFood`’s content to the browser’s console.

However, remember that the computer has not fully initialized the function’s `bestFood` variable yet. As such, the variable is currently in a temporal dead zone.

Therefore, the system’s attempt to access the variable returned a `ReferenceError`.

**Note:** After the `ReferenceError` returned, the computer stopped reading the function’s code. Therefore, JavaScript did not initialize the function’s `bestFood` variable with `"Vegetable Fried Rice"`.

## Wrapping It Up

Let’s see the previous walk-through of our program in one piece:

```js
let bestFood // 1. JavaScript parsed the first bestFood declaration

let myBestMeal // 2. the computer parsed myBestMeal variable declaration

bestFood = "Fish and Chips"; // 3. the computer initialized the bestFood variable

myBestMeal = function () {
  console.log(bestFood);
  let bestFood = "Vegetable Fried Rice";
}; // 4. JavaScript initialized myBestMeal variable

myBestMeal(); // 5. the computer invoked myBestMeal’s function

let bestFood // 6. JavaScript parsed the function’s bestFood declaration

console.log(bestFood); // 7. the computer parsed the function’s console.log statement

Uncaught ReferenceError // bestFood’s invocation returned an Error
```

You can see that JavaScript processed the program’s declarations before other code.

The parsing of declarations before other code in a program is what we call “hoisting”.

### References

https://www.freecodecamp.org/news/javascript-temporal-dead-zone-and-hoisting-explained/