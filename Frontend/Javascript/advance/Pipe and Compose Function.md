
Functional programming’s been quite the eye-opening journey for me. This post, and posts like it, are an attempt to share my insights and perspectives as I trek new functional programming lands.

[Ramda’s](http://ramdajs.com/) been my go-to FP library because of how much easier it makes functional programming in JavaScript. I highly recommend it.

### Pipe

The concept of `pipe` is simple — it combines `n` functions. It’s a pipe flowing left-to-right, calling each function with the output of the last one.

Let’s write a function that returns someone’s `name`.

```js
getName = (person) => person.name;
getName({ name: 'Buckethead' });
// 'Buckethead'
```

Let’s write a function that uppercases strings.

```js
uppercase = (string) => string.toUpperCase();
uppercase('Buckethead');
// 'BUCKETHEAD'
```

So if we wanted to get and capitalize `person`'s name, we could do this:

```js
name = getName({ name: 'Buckethead' });
uppercase(name);
// 'BUCKETHEAD'
```

That’s fine but let’s eliminate that intermediate variable `name`.

```js
uppercase(getName({ name: 'Buckethead' }));
```

Better, but I’m not fond of that nesting. It can get too crowded. What if we want to add a function that gets the first 6 characters of a string?

```js
get6Characters = (string) => string.substring(0, 6);
get6Characters('Buckethead');
// 'Bucket'
```

Resulting in:
```js
get6Characters(uppercase(getName({ name: 'Buckethead' })));
// 'BUCKET';
```

Let’s get really crazy and add a function to reverse strings.

```js
reverse = (string) =>
  string
    .split('')
    .reverse()
    .join('');

reverse('Buckethead');
// 'daehtekcuB'
```

Now we have:

```js
reverse(get6Characters(uppercase(getName({ name: 'Buckethead' }))));
// 'TEKCUB'
```

It can get a bit…much.

### Pipe to the rescue!

Instead of jamming functions within functions or creating a bunch of intermediate variables, let’s `pipe` all the things!

```js
pipe(
  getName,
  uppercase,
  get6Characters,
  reverse
)({ name: 'Buckethead' });
// 'TEKCUB'
```

Pure art. It’s like a todo list!

Let’s step through it.

For demo purposes, I’ll use a `pipe` implementation from one of [Eric Elliott](https://medium.com/@_ericelliott)’s [functional programming articles](https://medium.com/javascript-scene/reduce-composing-software-fe22f0c39a1d).

```js
pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x);
```

I love this little one-liner.

Using _rest_ parameters, [see my article on that](https://medium.com/@yazeedb/how-do-javascript-rest-parameters-actually-work-227726e16cc8), we can pipe `n` functions. Each function takes the output of the previous one and it’s all _reduced_ ? to a single value.

And you can use it just like we did above.

```js
pipe(
  getName,
  uppercase,
  get6Characters,
  reverse
)({ name: 'Buckethead' });
// 'TEKCUB'
```

I’ll expand `pipe` and add some debugger statements, and we’ll go line by line.

```js
pipe = (...functions) => (value) => {
  debugger;

  return functions.reduce((currentValue, currentFunction) => {
    debugger;

    return currentFunction(currentValue);
  }, value);
};
```

![1*jqrKgVeO-raAUJjuN-adug](https://cdn-media-1.freecodecamp.org/images/1*jqrKgVeO-raAUJjuN-adug.png)

Call `pipe` with our example and let the wonders unfold.

![1*rqi22p06rTtc2m0k1yHrRw](https://cdn-media-1.freecodecamp.org/images/1*rqi22p06rTtc2m0k1yHrRw.png)

Check out the local variables. `functions` is an array of the 4 functions, and `value` is `{ name: 'Buckethead' }`.

Since we used _rest_ parameters, `pipe` allows any number of functions to be used. It’ll just loop and call each one.

![1*UjM5plW8s--8chfQQg3cMg](https://cdn-media-1.freecodecamp.org/images/1*UjM5plW8s--8chfQQg3cMg.png)

On the next debugger, we’re inside `reduce`. This is where `currentValue` is passed to `currentFunction` and returned.

We see the result is `'Buckethead'` because `currentFunction` returns the `.name` property of any object. That will be returned in `reduce`, meaning it becomes the new `currentValue` next time. Let’s hit the next debugger and see.

![1*sEcE5tBFSpCzJZrKz8mmEQ](https://cdn-media-1.freecodecamp.org/images/1*sEcE5tBFSpCzJZrKz8mmEQ.png)

Now `currentValue` is `‘Buckethead’` because that’s what got returned last time. `currentFunction` is `uppercase`, so `'BUCKETHEAD'` will be the next `currentValue`.

![1*Va6taGFU8dSyhz1wLVMWMQ](https://cdn-media-1.freecodecamp.org/images/1*Va6taGFU8dSyhz1wLVMWMQ.png)

The same idea, pluck `‘BUCKETHEAD’`'s first 6 characters and hand them off to the next function.

![1*YaI1oxsZW5qZZUC146DYoQ](https://cdn-media-1.freecodecamp.org/images/1*YaI1oxsZW5qZZUC146DYoQ.png)

`reverse(‘.aedi emaS’)`

![1*moIMQxr82r2Z8IuXwuZfKQ](https://cdn-media-1.freecodecamp.org/images/1*moIMQxr82r2Z8IuXwuZfKQ.png)

And you’re done!

```js

// Functional Programming
// Often uses pipe and compose = higher order functions

/* A higher order function is any function which takes a function as an argument,
returns a function, or both. */

// Start with small unary (one parameter) functions

const add2 = (x) => x + 2;
const subtract1 = (x) => x - 1;
const multiplyBy5 = (x) => x * 5;

// Making our own pipe functions

/* Note: Ramda.js and lodash libraries have their own built-in compose and pipe functions.

lodash calls pipe "flow". */

/* The higher order function "reduce" takes a list of values and applies a function to each of those values, accumulating a single result. */

/* To do the same as compose, but read from left to right... we use "pipe".
It is the same except uses reduce instead of reduceRight. */

const pipe = (...fns) => (val) => fns.reduce((prev , fn) => fn(prev) , val);

const pipeResult = pipe(add2 , subtract1, multiplyBy5)(5);
console.log(pipeResult);

// You will often see the functions on separate lines
const pipeResult2 = pipe(
add2,
subtract1,
multiplyBy5
)(6);

console.log(pipeResult2);

/* This is a "pointer free" style where you do not see the unary parameter
passed between each function */

  
// example with a 2nd parameter
const divideBy = (divisor, num) => num / divisor;

const pipeResult3 = pipe(
add2,
subtract1,
multiplyBy5,
x => divideBy(2, x)
)(5);

console.log(pipeResult3);
// or you could curry the divideBy function for a custom unary function:

const divBy = (divisor) => (num) => num / divisor;
const divideBy2 = divBy(2); // partially applied

const pipeResult4 = pipe(
add2,
subtract1,
multiplyBy5,
divideBy2
)(5);

console.log(pipeResult4);


const lorem = `Lorem ipsum dolor sit amet, consectetur adipiscing elit,
sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut
aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum
dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.`;
  
const splitOnSpace = (string) => string.split(' ');
const count = (array) => array.length;
  
const wordCount = pipe(
splitOnSpace,
count
);

console.log(wordCount(lorem));

// The pipe function is reusable
const egbdf = "Every good boy does fine.";
console.log(wordCount(egbdf));


// Combine Processes: Check for palindrome
const pal1 = "taco cat";
const pal2 = "UFO tofu";
const pal3 = "Dave";

const split = (string) => string.split('');
const join = (string) => string.join('');
const lower = (string) => string.toLowerCase();
const reverse = (array) => array.reverse();

const fwd = pipe(
splitOnSpace,
join,
lower
);

  
const rev = pipe(
fwd,
split, // Split the string and returns array
reverse, // Reverse the array
join // Join characters to string
);


console.log(fwd(pal1) === rev(pal1));
console.log(fwd(pal2) === rev(pal2));
console.log(fwd(pal3) === rev(pal3));

// Clone / Copy functions within a pipe or compose function
// 3 approaches:

// 1) Clone the object before an impure function mutates it
const scoreObj = { home : 0, away : 0};

const shallowClone = (obj) => Array.isArray(obj) ? [...obj] : { ...obj };

const incrementHome = (obj) => {
	obj.home += 1; // mutation
	return obj;
}

  

const homeScore = pipe(

shallowClone,
incrementHome

// another function,

// and another function, etc

);

console.log(homeScore(scoreObj));
console.log(scoreObj);
console.log(homeScore(scoreObj) === scoreObj);

// Positive: Fewer function calls
// Negative: Create impure functions and testing difficulties

  

// 2) Curry the function to create a partial that is unary

let incrementHomeB = (cloneFn) => (obj) => {

const newObj = cloneFn(obj);
newObj.home += 1; // mutation
return newObj;

}

// Creates the partial by applying the first argument in advance
incrementHomeB = incrementHomeB(shallowClone);

const homeScoreB = pipe(
incrementHomeB
)

console.log(homeScoreB(scoreObj));
console.log(scoreObj);

// Positive: Pure function with clear dependencies
// Negative: More calls to the cloning function


// 3) Insert the clone function as a dependency

const incrementHomeC = (obj, cloneFn) => {

const newObj = cloneFn(obj);
newObj.home += 1; // mutation
return newObj;

}

const homeScoreC = pipe(
	x => incrementHomeC(x, shallowClone)
);


console.log(homeScoreC(scoreObj));
console.log(scoreObj);

// Positive: Pure function with clear dependencies
// Negatives: Non-unary functions in your pipe / compose chain
```

### What about compose()?

It’s just `pipe` in the other direction.

So if you wanted the same result as our `pipe` above, you’d do the opposite.

```js
compose(
  reverse,
  get6Characters,
  uppercase,
  getName
)({ name: 'Buckethead' });
```

Notice how `getName` is last in the chain and `reverse` is first?

Here’s a quick implementation of `compose`, again courtesy of the Magical [Eric Elliott](https://medium.com/@_ericelliott), from [the same article](https://medium.com/javascript-scene/reduce-composing-software-fe22f0c39a1d).

```js
compose = (...fns) => (x) => fns.reduceRight((v, f) => f(v), x);
```



```js
// Functional Programming

  

// Often uses pipe and compose = higher order functions

  

/* A higher order function is any function which takes a function as an argument,

returns a function, or both. */

  

// Here's how a "compose" function works:

  

// Start with small unary (one parameter) functions

const add2 = (x) => x + 2;

const subtract1 = (x) => x - 1;

const multiplyBy5 = (x) => x * 5;

  

// Notice how the functions execute from inside to outside & right to left

const result = multiplyBy5(subtract1(add2(4)));

console.log(result);

  

// The above is NOT a compose function.

  

// Making our own compose and pipe functions

  

/* Note: Ramda.js and lodash libraries have their own built-in compose and pipe functions.

lodash calls pipe "flow". */

  

/* The higher order function "reduce" takes a list of values and applies a function to each of

those values, accumulating a single result. */

  

/* To get the compose order from right to left as we see with nested function calls in our example

above, we need reduceRight... */

  

const compose = (...fns) => val => fns.reduceRight((prev, fn) => fn(prev), val);

  

const compResult = compose(multiplyBy5, subtract1, add2)(4);

console.log(compResult);
```

### References

https://www.freecodecamp.org/news/pipe-and-compose-in-javascript-5b04004ac937/

https://www.youtube.com/watch?v=kclGXphtmVg&list=PL0Zuz27SZ-6N3bG4YZhkrCL3ZmDcLTuGd&index=9