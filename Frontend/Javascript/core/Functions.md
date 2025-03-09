Refer to docs - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions

Functions are the cornerstone in JavaScript serving as reusable functions. There are different ways to create a function.

In this lesson we'll explore function-declaration syntax and function-expression syntax.

Function declaration syntax

```js
function sayHi() {
    console.log('Hi');
}

sayHi();
```

Function expression syntax

End the curly braces with a semi-colon and utilize an anonymous function.

```js
const sayHello = function() {
    console.log('Hello');
};

sayHello();

const greeting = function sayBye() {
    console.log('Bye');
}

greeting();
```

#### This Keyword

The 'this' keyword is crucial because it refers to the object that is currently executing the function or method.

The value of 'this' depends on the context in which a function is called, not where it's declared.

```js
const course = {
    name: 'JavaScript for Beginners',
    start() {
        console.log(this.name);
    }
}

course.start();
```

In a function, (meaning not a method defined for an object), the this keyword will reference the global object.

In browsers it's the window object and in the Node.js it's the global object.

```js
function startVideo() {
    // This outputs the global object in Node.js
    // This would output the window object in web browsers
    console.log(this);
    /*
    <ref *1> Object [global] {
  global: [Circular *1],
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function: setTimeout] {
    [Symbol(nodejs.util.promisify.custom)]: [Getter]
  },
  queueMicrotask: [Function: queueMicrotask],
  performance: Performance {
    nodeTiming: PerformanceNodeTiming {
      name: 'node',
      entryType: 'node',
      startTime: 0,
      duration: 56.27883338928223,
      nodeStart: 2.5072078704833984,
      v8Start: 5.197333335876465,
      bootstrapComplete: 30.964332580566406,
      environment: 16.188833236694336,
      loopStart: -1,
      loopExit: -1,
      idleTime: 0
    },
    timeOrigin: 1735555048808.809
  },
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function: setImmediate] {
    [Symbol(nodejs.util.promisify.custom)]: [Getter]
  }
}
    */
}
```

Arrow functions don't have their own 'this' context. Instead they inherit this from their parent scope at the time they are defined.

This is because when using arrow functions, the 'this' keyword inherits from its parents scope.
So in this case, it would be the global object (in Node.js).

Since the global object doesn't have a name property, then this value will be undefined.

```js
const newCourse = {
    name: 'ES6 syntax',
    start: () => {
	    console.log(this); // {}
        console.log(this.name); // undefined
    }
};

course.start(); // the output is undefined
```

You can explicitly set the value of 'this' using bind. So bind returns a new function with it bound to a specific object, but doesn't immediately invoke it.

```js
function introduce(language) {
    console.log(this.name + ' teaches ' + language);
}

const instructor = { name: 'Steven' };
const introduction = introduce.bind(instructor);

introduction('JavaScript'); // the output is 'Steven teaches JavaScript'
```

So the 'this' keyword in JavaScript plays a critical role in determining the context of function execution. Its value is determined by the execution context or explicitly bound using bind.

#### Hoisting - Refer to Temporal Dead Zone

**Hoisting** refers to JavaScript giving higher precedence to the declaration of variables, classes, and functions during a program’s execution.

The function declaration syntax and function expression syntax differ because of the concept of hoisting.

Hoisting is a process where the JavaScript engine moves all function declarations to the top of their enclosing scope.

With function declaration syntax, we can call/invoke a function before it is defined due to hoisting.

```js
add(2, 2);

function add(num1, num2) {
    return num1 + num2;
}
```

Hoisting doesn't apply with function expression syntax, so you cannot call/invoke a function before it is defined.

```js
multiply(2, 3); // ReferenceError: Cannot access 'multiply' before initialization

let multiply = function(num1, num2) {
    return num1 * num2;
}

console.log(multiply(2, 3)); // 6
```

#### Arguments

```js
function multiply(num1, num2){
    return num1 * num2;
}

console.log( multiply(2, 2) ); // 4
```

But if we were to call this function and only supply one argument.

If we don't supply an argument, then that parameter will be assigned the value of undefined.

```js
let multiply = function(num1, num2) {
	console.log(num2); // undefined
	return num1 * num2;
}
console.log( multiply(2) ); // NaN
```

You could also pass in more arguments than there are parameter variables defined.

To access those arguments, you could use the built in arguments object.

Every function in JavaScript has access to a special function named arguments.
So we just need to iterate over the arguments object.

```js
function add(num1, num2) {
    console.log(arguments); // [Arguments] { '0': 1, '1': 2, '2': 3, '3': 4 }
    let product = 1;
    for (const num of arguments)
        product *= num;

    return product;
}

console.log( add(1, 2, 3, 4) ); // 24
```

#### Rest Operator

Previously we covered the spread operator which is used for cloning objects in JavaScript.

So the spread operator allows us to easily copy properties and methods from one object to another. Making a clone of the object.

```js
let course = {
    name: 'JavaScript for Beginners',
    duration: '3 hours'
};

let newCourse = { ...course };
console.log('newCourse', newCourse);
```

We could also modify the properties for our own custom use.

```js
newCourse = { 
    ...course,
    name: 'JavaScript Pro'
};

console.log('newCourse', newCourse);
```

Now a similar syntax can be used in the context of functions. So when we use, ..., in functions, we refer to it as the rest operator. So unlike the spread operator which expands an array or object, the rest operator allows us to gather a varying number of arguments into a single array parameter.

This is particularly useful when we want a function to accept an indefinite number of arguments.

The name of the parameter doesn't matter.

```js
function multiply(...args) {
    return args.reduce((accumulator, currentValue) => accumulator * currentValue, 1);
}

console.log( multiply(1, 2, 3, 4) ); // results in the value of 24
```

So the parameter numbers is an array containing a varying number of values based on the arguments supplied.

```js
function multiplier(multiplierValue, ...numbers) {
    return numbers.map(number => number * multiplierValue);
}

console.log( multiplier(2, 1, 2, 3, 4) ); // [ 2, 4, 6, 8 ]
```

#### Default Parameters

Default parameters enhance function flexibility.

```js
function writeCode(language) {
    console.log(`Write code in ${language}`);
}

writeCode('JavaScript');
writeCode('C#');
```

But what if we were to call writeCode() without any arguments. The parameter, language, would have the value of undefined by default.

```js
writeCode();
```

We can assign a default value to parameters, ensuring that our function behaves predictably
even when specific arguments aren't provided.

```js
function writeProgram(language = 'JavaScript') {
    console.log(`Write code in ${language}`);
}
```

If you set a default parameter, then all parameters that follow must also be assigned a default value.

```js
function codeDetails(language = 'JavaScript', tool = 'VS Code') {
    console.log(`Writing code in ${language} using ${tool}`);
}

// This enables us to call/invoke our function with flexibility
codeDetails();
codeDetails('Python');
codeDetails('C#', 'Visual Studio');

function createUser(name, role = 'guest', status = 'active') {
    console.log(`User: ${name}, Role: ${role}, Status: ${status}`);
}

createUser('Steven');
createUser('Alice', 'admin', 'active');
```

#### getters and setters

Getters and setters within JavaScript, enhance how you interact with object properties.

```js
let course = {
    name: 'JavaScript for Beginners',
    duration: '3 hours',
    details() {
        return `${this.name} is ${this.duration}`;
    }
};

console.log(`${course.name} is ${course.duration}`);
console.log( course.details() );
```

By using getters and setters, these enable us to define custom logic for reading and writing properties. This enables us to use methods as if they were regular properties as a means of encapsulation. So they act like virtual properties.

```js
course = {
    name: 'JavaScript for Beginners',
    duration: '3 hours',
    get details() {
        return `${this.name} is ${this.duration}`;
    },
    set details(value) {
        let parts = value.split(' is ');

        this.name = parts[0];
        this.duration = parts[1];
    }
};

console.log( course.details ); // JavaScript for Beginners is 3 hours

course.details = 'JavaScript Pro is 10 hours';

console.log( course.details ); // JavaScript Pro is 10 hours
```

#### Try and catch

Refer to MDN docs :

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch

The powerful concept of error handling is needed for all JavaScript applications that utilize backend APIs.

This is done through try-catch blocks.

Using try-catch blocks enable us to catch possible exceptions so that we can gracefully handle errors without our programs crashing/terminating.

```js
let course = {
    name: 'JavaScript for Beginners',
    duration: '3 hours',
    get details() {
        return `${this.name} is ${this.duration}`;
    },
    set details(value) {
        if (typeof value !== 'string') {
            // Here we are throwing an exception.
            throw new Error(`Value, ${value} is not a string`);
        }

        let parts = value.split(' is ');

        this.name = parts[0];
        this.duration = parts[1];
    }
};

try {
    // Within try blocks, would place code that could potentially throw exceptions.
    course.details = 42;
} catch (e) {
    console.log(`Caught an error: ${e.message}`);
}
```

#### Error handling

Refer to MDN docs :

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error

Types of Error:

1. Syntax Error
2. Type Error

We can declare a variable without using let, var and const if we are not using use strict.

```js
variable = "Sanjay";
console.log(variable); // Sanjay
```

However, if we use 'use strict' we will get Reference Error.

```js
'use strict';
variable = "Sanjay"; // ReferenceError: variable is not defined
console.log(variable); 
```

```js
'use strict';
const variable = "Sanjay";
console.log(variable); // Sanjay
```

As you know, const variable can't be reassigned. We will get TypeError: Assignment to constant variable.

```js
const uname = "Dave";
uname = "Joe"; // TypeError: Assignment to constant variable.
```

Creating Custom Error

```js
const makeError = () => {

	let i = 1;
	while(i <= 5) {
	try {
		if( i % 2 !== 0) {
			throw new customError("Odd number!"); // Custom Error
			//throw new Error("Odd number!");
		}
		console.log("Even number!");
	
	} catch(err) {
		console.error(err.stack); // customError: Odd number!
	} finally {
		console.log("finally called");
		i++;
	}
	}
}

makeError();

function customError(message) {
	this.message = message;
	this.name = "customError";
	this.stack = `${this.name}: ${this.message}`;
}
```

#### Local vs Global Scope

Refer to MDN docs:

https://developer.mozilla.org/en-US/docs/Glossary/Scope

https://developer.mozilla.org/en-US/docs/Glossary/Global_scope

Global scope, so a variable declared outside any function block or conditional has a global scope.  Meaning that it is accessible from any part of the code after its declaration.

This variable is declared in the global scope.

```js
const name = 'Steven';
console.log(name);
```

#### Local Scope

Local scope refers to variables declared within blocks, functions, or conditionals.
So these variables are only accessible within the confines of their curly braces.

```js
{
    let lastName = 'Garcia';
    console.log(lastName);
}

// however we can't access the variable, lastName, outside of the code block.

function greet() {
    // So the variable, message, is a local variable
    const message = 'Hello World';
    console.log(message);
}

greet();

// can't access the variable, message, outside of the function block.
```

#### Let vs Var

The differences between the let and var keyword are a commonly asked interview question.
In JavaScript versions prior to ES6 (ECMAScript 2015), the var keyword was used to declare variables.

The var keyword is function-scoped. Meaning that it is available anywhere within a function.

```js
function display() {
    for (var i = 0; i < 5; i++) {
        console.log(i);
    }
    // we can still access the variable, i, outside of the block it is defined in
    console.log(i);
}
```

The let keyword introduces block-scoping to JavaScript. This means that a variable declared with let, is only accessible within the block it's defined in.

We cannot access the variable, i, outside of the block it is defined it since we declared it with the let keyword and it is therefore, block-scoped if we tried to access it, then it would result in a ReferenceError.

```js
function displayNumbers() {
    for (let i = 0; i < 5; i++) {
        console.log(i);
    }
    console.log(i); // Reference Error
}
```


![[20241225114640.png]]



