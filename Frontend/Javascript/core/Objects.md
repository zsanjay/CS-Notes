Refer to MDN docs - https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Object_basics
#### Object Literals

Objects are a way to store key-value pairs.
They allow us to group together state and behavior that's highly related.
The benefit is we can make our code cohesive (related things are grouped together) and organized.

#### Encapsulation

Encapsulation involves grouping together data and the methods that manipulate that data into a single unit. While hiding or abstracting away the internal details (state) from outside interference or misuse.

```js
const dog = {
    name: 'Max',
    breed: 'Dachshund',
    age: 5,
    weightInPounds: 12,
    eat: function() {
        console.log('Chomp!');
    },
    bark() {
        console.log('Woof!');
    }
};
```

#### Garbage Collection

In programming languages such as C or C++, when you create an object you have to explicitly allocate and deallocate memory.

But in programming languages such as JavaScript, C#, Java, or Python the languages have garbage collection to manage memory for us.

### Math

In JavaScript, we have a built-in class called Math. This has static helper methods to help us do mathematical calculations. It is often utilized in programming interviews.

Refer to the documentation at developer.mozilla.org since there are many built in methods.
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math

Some of the most commonly used built-in methods:

```js
console.log( Math.round(3.14) ); // will round the number to 3 

console.log( Math.round(3.6) ); // will round the number to 4

console.log( Math.floor(4.6) ); // 4

console.log( Math.ceil(4.2) ); // 5

console.log( Math.max(1, 2) ); // 2

console.log( Math.max(1, 2, 3, 4, 5)); // 5

console.log( Math.min(1, 2, 3) ); // 1

console.log( Math.pow(2, 3) ); // 8

console.log( Math.sqrt(25) ); // 5

console.log( Math.random() ); // returns a decimal value between 0 and 1

```


![[20241224185407.png]]

### Code Challenge

#### Write code that will return a random letter from your name.

```js
function getRandomCharacterFromName(name) {

let index = Math.floor(Math.random() * name.length);

return name.charAt(index);
}
```

#### String 

Refer to the documentation at mozilla.developer.org:

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String

```js
// We have string literals
const name = 'Steven';
console.log(typeof name); // 'string'

// can also make a string using the string object
const anotherName = new String('Joe');
console.log(typeof anotherName); // 'object'
```

We can access the methods of strings using dot notation or bracket notation.

The JavaScript engine will internally wrap the string literal with the string object.
There are built in methods for strings that allow us to operate and manipulate strings.

```js
let sentence = 'A new sentence';

const doesIncludeNew = sentence.includes('new');
console.log('doesIncludeNew', doesIncludeNew); // true

// can access characters by index value, where the first character of a string is at the index 0
console.log(sentence[3]); // 'e'

const startsWithA = sentence.startsWith('A');
console.log('startsWithA', startsWithA); // true

const endsWithA = sentence.endsWith('A');
console.log('endsWithA', endsWithA); // false

let updatedSentence = sentence.replace('new', 'short');
console.log(updatedSentence); // 'A short sentence'

let input = ' username@gmail.com ';
console.log( trim(input) ); // 'username@gmail.com', so trim() removes spaces at the beginning and the end

// split(), will split the sentence into an array
const wordsInSentence = sentence.split(' '); // so split on the empty space
console.log(wordsInSentence);

let email = 'TesTEmail@gmail.com';
console.log( email.toLowerCase() );
console.log( email.toUpperCase() );

```

### Template Literals

In previous lessons when working with strings, we have used either single quotes, '', or double quotes, "".

We have another way to create a string literal and that is with backticks, `We would call this a template literal`.

The benefit is that it allows us to use string interpolation rather than string concatenation.

```js
let firstName = 'Steven';
let lastName = 'Garcia';

// string concatenation
let fullName = firstName + ' ' + lastName;
```

String interpolation with a template literal. ${} acts as a placeholder and within, we can specify an expression. An expression is any code that would return some value.

```js
let fullName = `${firstName} ${lastName}`;
```

#### Date object

In JavaScript we have a built in Date object which stores the date and time. It also provides methods for data and time management.

We would use the Date object most commonly to store creation and modification times
for a resource in our database. The Date object can be initialized many different ways.

Refer to the documentation at developer.mozilla.org:

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date

```js
let now = new Date();
console.log( now ); // 2024-12-29T14:50:37.632Z

// passing a number as an argument, will represent the number of milliseconds since Jan 1, 1970.
let Jan01_1970 = new Date(0); // 0 seconds since Jan 1, 1970
console.log(Jan01_1970); // 1970-01-01T00:00:00.000Z

// One day before Jan01_1970
let Dec31_1969 = new Date(-24 * 3600 * 1000);
console.log(Dec31_1969);

const dateOne = new Date('December 25, 2024 16:08');

const Jan1_2024 = new Date(2024, 0, 1); // Jan 1, 2024

console.log( now.getFullYear() ); // 2024

// Represented as a number,
// so 0 = Jan, 1 = Feb, etc.
console.log( now.getMonth() ); // 11

/** Gets the difference in minutes between the time on the local computer and Universal Coordinated Time (UTC). */
getTimezoneOffset(): number;

console.log( now.getTimezoneOffset() ); // -240
```

#### Factory Functions

If we were to create another object with the same properties and methods as the dog object, it would require that we copy and paste. Copy and pasting is a sign of code duplication which is something that we want to avoid in our programs.

This makes our code harder to read, more susceptible to bugs, and more difficult to modify.

```js
const dog = {
    name: 'Max',
    breed: 'Dachshund',
    age: 5,
    weightInPounds: 12,
    eat: function() {
        console.log('Chomp!');
    },
    bark() {
        console.log('Woof!');
    }
};
```


One way that we can create another dog object, it to use a factory function. This refers to a function, named with the camelCase naming convention, which returns an object literal.

The details are within the factory function and we pass arguments to the factory function's parameter variables. The arguments are used to create that custom object.

```js
function getDog(name, breed, age, weightInPounds) {
    return {
        name,
        breed,
        age,
        weightInPounds,
        eat: function() {
            console.log(this.name + ': Chomp!');
        },
        bark() {
            console.log(this.name + ': Woof!');
        }
    };
}

const anotherDog = getDog('Marley', 'Chocolate Lab', 3, 60);
```

Another use case of factory function, it is used before ES6 to make private property or inaccessible.

```js

function pizzaFactory(pizzaSize) {

const crust = "original";

const size = pizzaSize;

return {

bake : () => console.log(`Baking a ${size} ${crust} crust pizza.`)

};

}

const myPizza = pizzaFactory("small");
myPizza.bake();

```

#### Constructor Functions

We know that the object literal syntax creates one object. We can easily use factory functions and have it return an object.

Factory functions provide an efficient and reusable way to create new objects which the same state and behavior. (properties and methods)

However factory functions are not the common way of achieving this, rather we use constructor functions.

Constructor functions construct the initial state of an object. We name our constructor functions using PascalNotation. We would name our constructor function as a noun, rather than a verb. Then we utilize the 'this' keyword, as in 'this current object' to set the state and behavior. (properties and methods).

```js
function Dog(name, breed, age, weightInPounds) {
    // this is done internally
    // this = {};

    // Add properties to this object
    this.name = name;
    this.breed = breed;
    this.age = age;

    // Add methods to this object

    this.eat = function() {
        console.log(this.name + ': Chomp!');
    },

    this.bark = function() {
        console.log(this.name + ': Woof!');
    }

    // this is done internally
    // return this;
}

```

To call the constructor function, we use a special keyword, new. So we would say that we, instantiated a new Dog object in memory. The new keyword, creates an empty JavaScript object.
It then sets the new keyword to reference this object in memory. Then it returns the this keyword. 
The goal of a constructor function is to instantiate/initialize an object with an initial state. (set by the arguments passed to the parameter variables).

```js
const anotherDog = new Dog('Marley', 'Chocolate Lab', 3, 60);
```

#### Objects are dynamic

Objects in JavaScript are dynamic.
Meaning that we can add properties or methods at any time.

The const keyword used with an object, means that we can't reassign it.But we can still change/mutate the properties and methods the object that it references.

```js
const person = {
    name: 'Steven'
};

console.log(person);

// We can add properties on the fly with dot notation (member notation)
person.favoriteFood = 'tacos';

console.log(person);

// Could also use bracket notation
person['favoriteIceCream'] = 'chocolate';

console.log(person);

// You could delete properties with the delete keyword
delete person.favoriteIceCream;

console.log(person);

person.eat = function() {
    console.log('start eating');
}

person.eat();
```


#### Constructor Property

Every object in JavaScript has a constructor function.

```js
const person = {
    name: 'Steven'
};

console.log( person.constructor ); // [Function: Object]
```

Internally JavaScript sees this as let newObj = new Object();
so object literal syntax is syntactic sugar.

```js
let newObj = {}; // Object literal

// Internally 
let newObj = new Object();

new String(); // 'Sanjay' - string literal
new Boolean(); // true, false - boolean literal
new Number(); // 1, 2, 3 - number literal
```

#### Functions are Objects.

Functions are objects in JavaScript. So this function is an object in memory

```js
function add(num1, num2) {
    return num1 + num2;
}
```

So now n references this object in memory, the function named add. 

```js
const n = add;

console.log(n(2, 2));
```

So the function named add, has members. Meaning that it has properties and methods.

This will output the number of parameters that the add function has.

```js
console.log( add.length ); // 2
```

This constructor function is an object in memory.

```js
function Programmer(name) {
    this.name = name;
    this.writeCode = function() {
        console.log('Code in JavaScript');
    }
}

console.log( Programmer.length ); // 1 parameter
console.log( Programmer.constructor ); // references the constructor function [Function: Function]
```

To further demonstrate how functions are objects in JavaScript.

```js
const ProgrammerFunc = new Function('name', `
    this.name = name;
    this.writeCode = function() {
        console.log('Code in JavaScript');
    }
`);

const newProgrammer = new ProgrammerFunc('Steven');
newProgrammer.writeCode(); // Code in JavaScript
```

#### Value vs Reference Types 

We have eight different data types in JavaScript, which include 7 primitive data types, which include number, string, boolean, BigInt, undefined, null, and Symbol

The eight data type are objects. Arrays and functions fall into the object category

The reason we differentiate the data types is because of how they are allocated in memory.

So when working with primitive values, they are passed by copy.

Meaning that if you were to assign a variable to an existing variable (containing a primitive value).

Then that new variable would be given a copy of that primitive value. Then changing one of the variables won't affect the other variable.

This is because they are two different variables and are assigned to different memory addresses.

```js
let a = 10;
let b = a;

a = 20;

console.log(a); // 20
console.log(b); // 10

// Now consider references types
a = { value: 20 };
b = a;

a.value = 100;

console.log(a); // 100
console.log(b); // 100
```

So they have the same value, this is because objects are passed by reference.\

Both of the variables a and b are assigned to the same object in memory (the same memory address).

So to summarize:

Primitive values are copied by their value.
Objects are copied by reference.

#### Enumerating Properties of an object.

We have the for-of loop for iterating over an array.
We have the for-in loop to iterate over the keys of an object.

```js
let numbers = [1, 2, 3, 4, 5];

for (const elements of numbers)
    console.log(elements);

const dog = {
    name: 'Max',
    age: 5,
    eyeColor: 'blue'
};

for (const key in dog) 
    console.log(dog[key]);
```

We have another syntax for enumerating over the keys and values of an Object.

This returns the keys of the object as an array.

```js
const keys = Object.keys(dog); // ['name', 'age', 'eyeColor']

for (const key of Object.keys(dog))
    console.log(key);
```

This returns the values of the object as an array.

```js
const values = Object.values(dog); // ['Max', 5, 'blue']
for (const value of Object.values(dog))
    console.log(value);
```

This returns the key-value pairs of the object as an array.
So each element will be an array itself, [key, value]

```js
const entires = Object.entries(dog); // [['name', 'Max'], ['age', 5], ['eyeColor', 'blue']];

for (const entry of Object.entries(dog))
    console.log(`Key: ${entry[0]} => Value: ${entry[1]}`);
```

#### Cloning an object.

```js
let a = { value: 10 };
let b = a;
```

Since they both reference the same object, if you update one variable, it will be reflected in the other variable.

```js
b.value = 20;

console.log(a); // {value : 20}
console.log(b); // {value : 20}
```

So if you wanted to have it be the same where if you change one property it doesn't affect the other variable. Then you would need to create a clone of that object.

1. Object.assign method

The first argument (b) is the object you want to copy to. (the target object).
Then you could pass one or more source objects whose properties and methods will be copied to the target object.(a)

So this creates a copy of the object so changing one won't affect the other.

```js
Object.assign(b, a);
```

We have a more modern syntax to accomplish this, using the spread operator.

2. The spread operator is represented with three dots.

So this creates a copy of the object so changing one won't affect the other.
```js
b = { ...a };
```

Refer - [[Shallow vs Deep Copy]]

#### More on Objects

```js

// Objects - key value pairs in curly braces.
const myObj = { name : "Sanjay" };


const anotherObj = {

alive : true,

answer : 42,

hobbies : ["Eat", "Sleep", "Code"],

beverage: {

morning : "Coffee",

afternoon : "Iced Tea"

},

action : function() {

return `Time for ${this.beverage.morning}`;

}

};

  
  

// dot notation

console.log(myObj.name);

console.log(anotherObj.alive);

  
  

console.log(anotherObj["alive"]);

  

console.log(anotherObj.beverage.morning);

  

console.log(anotherObj.action());

  
  

const vehicle = {

wheels : 4,

engine : function() {

return "Vrroooom!";

}

}

  

const truck = Object.create(vehicle);

truck.doors = 2;

console.log(truck);

console.log(truck.__proto__.engine());

console.log(truck.__proto__);

console.log(truck.wheels); // Inheritance

console.log(truck.engine());

  

const car = Object.create(vehicle);

car.doors = 4;

car.engine = function () {

return "Whoooosh!";

};

console.log(car.engine());

  
  

const tesla = Object.create(car);

console.log(tesla.wheels);

console.log(tesla.engine());

  

tesla.engine = () => "Shhhhh....";

  

console.log(tesla.engine());

  
  

const band = {

vocals: "Robert Plant",

guitar: "Jimmy Page",

bass : "John Paul Jones",

drums : "John Bonham"

};

  

// deleting the key value pair

delete band.drums;

console.log(band.hasOwnProperty("drums"));

  

// Print all keys of the Object

console.log(Object.keys(band));

  

// Print all values of the object

console.log(Object.values(band));

  

// for in loop

  

for(let job in band) {

console.log(`On ${job}, it's ${band[job]}!`);

}

  

// destructuring objects

  

const newBand = {...band , drums : "John Bonham"};

  

const { guitar : myVariable, bass : myBass} = newBand;

  

console.log(myVariable);

console.log(myBass);

  

const { vocals, guitar, bass, drums} = newBand;

console.log(guitar);

console.log(vocals);

console.log(drums);


function sings({vocals}) {

return `${vocals} sings!`;

}

console.log(sings(newBand));
```

