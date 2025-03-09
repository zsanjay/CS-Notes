
The var keyword is the previous keyword used to create a variable.
Declaring and assigning a variable on the same line is known as initialization

```js
var previousNamingConvention = 'Your First Name';
```

### let

The let keyword is the modern syntax to create a variable

```js
let firstName = 'Your First Name';
```

Variable declaration:

```js
let variableDeclaration;
```

Variable assignment. The equal sign, =, is known as the assignment operator.

```js
variableDeclaration = 'value';
```

You output to the console with console.log();

```js
console.log(variableDeclaration);
```


We use the let keyword for variables which hold values that can be changed.
### const

The const keyword is for values that can not be changed.

```js
const christmas_2024 = '12.25.2024';
console.log(christmas_2024);
```

If you tried to reassign the constant, then when you run it, 
JavaScript would throw an error saying **'TypeError: Assignment to constant variable.'**

```js
christmas_2024 = '12.26.2024';
```

May sometimes see constants named with the snake_case naming convention depending on the naming convention of your development team.

Also may see variables named with snake_case with the letters capitalized.

```js
const COLOR_GREEN = 'green';
```

Regardless of the naming convention, it's best to use descriptive names for your variables.
### In JavaScript, data can belong to two different categories.

### Primitive value types or Reference types.

Primitive types refer to simple, fundamental data.
### The 8 data types in JavaScript are

### string, number, BigInt, boolean, undefined, null, Symbol, and object.

#### string

There are 3 ways to create a string literal.

1. Initialize a string with single quotes.

```js
let favoriteFruit = 'strawberries';
```

2. Initialize a string with double quotes.

```js
let favoriteIceCream = "chocolate";
```

3.  Initialize a string with backticks, ``.

This creates a template literal which can be used for string interpolation.

```js
let favoriteProgrammingLanguage = `JavaScript`;
```

#### number

Refer to MDN Docs - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number

Assign to a number

```js
let numberOfDonuts = 12;
```

In JavaScript, the number data type, encompasses both integers and decimal values. 

```js
let pi = 3.14;
```

#### BigInt

BigInt, ends with the character n, and this is for very large numeric values.

```js
let veryLargeNumber = 54389759347634976346n;
```
#### Boolean

The boolean data type holds the value of true or false.

```js
let lovesCoding = true;
```

#### undefined

undefined is the default value of a variable if we do not assign it.

```js
let favoriteColor;
```

Outputting it will display undefined

```js
console.log(favoriteColor);
```

#### Note - Refer to temporal dead zone

#### null

The null data type, means value unknown or empty.

```js
favoriteFruit = null;
```
#### Symbol

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol

The Symbol data type, is used to create unique identifiers for objects.

To create a new primitive Symbol, you write `Symbol()` with an optional string as its description:

```js
const sym1 = Symbol();
const sym2 = Symbol("foo");
const sym3 = Symbol("foo");
```

The above code creates three new Symbols. Note that `Symbol("foo")` does not coerce the string `"foo"` into a Symbol. It creates a new Symbol each time:

```js
Symbol("foo") === Symbol("foo"); // false
```

The following syntax with the [`new`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) operator will throw a [`TypeError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError):

```js
const sym = new Symbol(); // TypeError
```

This prevents authors from creating an explicit `Symbol` wrapper object instead of a new Symbol value and might be surprising as creating explicit wrapper objects around primitive data types is generally possible (for example, `new Boolean`, `new String` and `new Number`).

If you really want to create a `Symbol` wrapper object, you can use the `Object()` function:

```js
const sym = Symbol("foo");
typeof sym; // "symbol"
const symObj = Object(sym);
typeof symObj; // "object"

```

Because symbols are the only primitive data type that has reference identity (that is, you cannot create the same symbol twice), they behave like objects in some way. For example, they are garbage collectable and can therefore be stored in [`WeakMap`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap), [`WeakSet`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet), [`WeakRef`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef), and [`FinalizationRegistry`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry) objects.

#### object

The object data type, is known as a reference data type.
So primitive values can only contain a single thing, such as a number or a string.
But objects can have more complex structures and hold key-value pairs.

```js
let course = {
    name: 'JavaScript for Beginners',
    hours: 3
};
```

#### typeof operator

Also have the typeof operator, which will return the type of the operand
The operand will be either a variable or a literal value.

```js
console.log(typeof 3); // this will output, 'number'

console.log(typeof favoriteFruit); // this will output, 'string'

console.log(typeof 109234532525n); // this wil output, 'bigint'

console.log(typeof 'taco'); // this will output, 'string'

console.log(typeof null); // this will output, 'object'
```

#### Important note

null, is not actually an object. This is from the early days of JavaScript, and it returns the value of 'object' for backwards compatibility.

-----------------------------------------------------------------------
### Dynamic Typing

JavaScript is dynamically typed.

Meaning that you can initialize a variable to a particular data type.Then you can reassign it later to a different data type.

Programming languages which are statically typed, such as Java or C#, do not allow you to do this.

```js
let firstName = 'Steven';
console.log(typeof firstName); // string

firstName = 100;
console.log(typeof firstName); // number

firstName = true;
console.log(typeof firstName); // boolean
```

#### Objects

Objects are reference types. (They are not a primitive value). Objects represent nouns. (person, place, or thing). They contain state or behavior, allowing you to group together related values.

#### Object Literals

Object Literals use curly braces. Here you would specify key-value pairs.

```js
let course = {
    name: 'JavaScript for Beginners',
    hours: 3,
};
```

You can access a property through dot notation.

```js
console.log(course.name);
```

You can also reassign the value.

```js
course.name = 'JavaScript Fundamentals';
console.log(course.name);
```

You can also access properties through bracket notation. You would typically use dot notation however bracket notation is used if you don't know the exact key/property you want to access until runtime.

```js
console.log(course['name']);
course['name'] = 'JavaScript 101';
console.log(course.name);
```

An example of how you would use bracket notation.

```js
let property = 'hours';
console.log(course[property]);
```

To demonstrate the different aspects of a JavaScript object. The key-value is referred to as a property of the JavaScript object.

```js
let obj = {
    key: 'value'
};
```

#### Array

Arrays are used to store lists of data. We use square brackets to create an array literal.

```js
let productColors = ['blue', 'green'];
console.log(productColors); // ['blue', 'green']
```

The elements of an array can be accessed by index. We begin counting the index from 0.
So to access the element of 'blue'.

```js
console.log(productColors[0]); // blue
```

Arrays can contain values of any data type. However we typically use arrays where all the elements are the same data type. 

```js
productColors[0] = 42;
console.log(productColors[0]); // 42
```

Arrays are objects. Objects consist of key-value pairs.
In the context of arrays, the keys are an index value. (a numeric value starting from 0)

The data type is an object.
```js
console.log(typeof productColors); // object
```

A useful property that you'll often use:
```js
console.log(productColors.length);
```

#### Functions

Functions are the building blocks of our applications. It allows us to group together statements to perform a task or calculate a value.

Function declaration syntax

```js
function sayHi() {
    console.log('Hi!');
}
```

Then we invoke/call the function.

```js
sayHi();
```

Can also define a parameter variable within the parenthesis.

```js
function sayHiPartTwo(name) {
    // Then we use string concatenation to use that variable.
    console.log('Hi! ' + name);
}
```

#### Types of functions

Functions are a building block in our programs. They enable us to group together a block of code, which is a set of statements defined within curly braces.

We create a function for one of two options:
1. The first option is to perform some action.
2. The second option is to calculate and return some value.

So not all functions require us to explicitly use the return keyword. By default if we don't supply an explicit return value, then JavaScript will have the function return the value undefined.

```js
function multiply(num1, num2) {
    return num1 * num2;
}

console.log( multiply(2, 2) );
```

If we don't specify a return keyword, then JavaScript will return undefined by default.

```js
function add(num1, num2) {
    const sum = num1 + num2;
}

console.log( add(3, 3) );
```

### References

https://github.com/stevenGarciaDev/javascript-for-beginners-notes/tree/main/2%20JavaScript%20Variables

https://www.youtube.com/watch?v=Zi-Q0t4gMC8

