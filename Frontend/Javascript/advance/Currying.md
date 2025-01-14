Currying in [JavaScript](https://builtin.com/software-engineering-perspectives/javascript) is a process in [functional programming](https://builtin.com/software-engineering-perspectives/functional-programming) in which you can transform a function with multiple arguments into a sequence of nesting functions. It returns a new function that expects the next argument inline.  

In other words, instead of a function taking all arguments at one time, it takes the first one and returns a new function, which takes the second one and returns a new function, which takes the third one, and so on, until all arguments have been fulfilled.

## What Is Currying in JavaScript?

Currying in JavaScript transforms a function with multiple arguments into a nested series of functions, each taking a single argument. Currying helps you avoid passing the same variable multiple times, and it helps you create a higher order function.

That is, when we turn a function call `sum(1,2,3)` into `sum(1)(2)(3)`. 

The number of arguments a function takes is also called `arity`.

```
function sum(a, b) {
    // do something
}
function _sum(a, b, c) {
    // do something
}
```

The function `sum` takes two arguments (two-arity function) and `_sum` takes three arguments (three-arity function).

Curried functions are constructed by chaining closures and by defining and immediately returning their inner functions simultaneously.

## Why Is Currying in JavaScript Useful?

1. Currying helps you avoid passing the same variable again and again.
2. It helps to create a higher order function.

Currying transforms a function with multiple arguments into a sequence/series of functions, each taking a single argument.

For example:

```
function sum(a, b, c) {
    return a + b + c;
}
sum(1,2,3); // 6
```

As you can see, this is a function with full arguments. Let’s create a curried version of the function and see how we would call the same function (and get the same result) in a [series of calls](https://builtin.com/software-engineering-perspectives/javascript-call-stack):

```
function sum(a) {
    return (b) => {
        return (c) => {
            return a + b + c
        }
    }
}
console.log(sum(1)(2)(3)) // 6
```

We could even separate this `sum(1)(2)(3)` to understand it better:

```
const sum1 = sum(1);
const sum2 = sum1(2);
const result = sum2(3);
console.log(result); // 6
```

Currying takes a function that receives more than one parameter and breaks it into a series of unary (one parameter) functions.

```js
// Currying can look like this:
const buildSandwich = (ingredient1) => {
	return (ingredient2) => {
		return (ingredient3) => {
			return `${ingredient1}, ${ingredient2}, ${ingredient3}`;
		}
	}
}


const mySandwich = buildSandwich("Becon")("Lettuce")("Tomato")

console.log(mySandwich);


// It works but thats getting ugly and nested the further we go

// Let's refector:

const buildSammy = ingred1 => ingred2 => ingred3 =>

`${ingred1}, ${ingred2}, ${ingred3}`;

  

const mySammy = buildSammy("turkey")("cheese")("bread");

console.log(mySammy);

  

// Another Example of a curried function
const multiply = (x, y) => x * y;
const curriedMultiply = x => y => x * y;

console.log(multiply(2 , 3));
console.log(curriedMultiply(2));
console.log(curriedMultiply(2)(3));

  
// Partially applied functions are a common use of currying
const timesTen = curriedMultiply(10);

console.log(timesTen);
console.log(timesTen(8));

  
// Another example
const updateElemText = id => content => document.querySelector(`#${id}`).textContent = content;

const updateHeaderText = updateElemText('header');

updateHeaderText("Hello Sanjay!");

  

// Decorator Pattern

// Another common use of currying is function composition

// Allows calling small functions in a specific order

const addCustomer = fn => (...args) => {
	console.log('saving customer info...');
	return fn(...args);
}

const processOrder = fn => (...args) => {
	console.log(`processing order #${args[0]}`)
	return fn(...args);
}
 
let completeOrder = (...args) => {
	console.log(`Order #${[...args].toString()} completed.`);
}


completeOrder = (processOrder(completeOrder));

console.log(completeOrder);

completeOrder = (addCustomer(completeOrder));

console.log(completeOrder);

completeOrder("1000");


// Above function can be written using nested functions
function addCustomerNested(...args) {
	return function processOrderNested(...args) {
		return function completeOrderNested(...args) {
// end
		}
	}
}

// Requires a function with a fixed number of parameters
const curry = (fn) => {
	return curried = (...args) => {
		if(fn.length !== args.length) {
			return curried.bind(null, ...args); // bind creates new function
		}
		return fn(...args);
	};
}


const sum = (x , y , z) => x + y + z;
console.log(curry(sum));


const currySum = curry(sum);
console.log(currySum(10)(20)(30));
```

### References

https://builtin.com/software-engineering-perspectives/currying-javascript#:~:text=What%20Is%20Currying%20in%20JavaScript,create%20a%20higher%20order%20function.

https://www.youtube.com/watch?v=I4MebkHvj8g&list=PL0Zuz27SZ-6N3bG4YZhkrCL3ZmDcLTuGd&index=4

https://dev.to/oculus42/making-curry-javascript-functional-programming-2d6e