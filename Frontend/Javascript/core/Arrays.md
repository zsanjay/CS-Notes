Arrays are a commonly used data structure. It provides a collection of elements (a list of items).

In JavaScript arrays, the index position stores an element that can be of any data type.
However in real applications, all elements of an array are usually of the same data type.

There are many built in methods for arrays, which enable you to modify, filter, and retrieve data, using clean and modern syntax.

Refer to docs - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array

```js
// Arrays
const myArray = [];


// add elements to an array
myArray[0] = "Sanjay";
myArray[1] = 1001;
myArray[2] = false;


// refer to an array
console.log(myArray);

// Length property
console.log(myArray.length);


// Last element in an array
console.log(myArray[myArray.length - 1]);


// add an element to the end
myArray.push("school");


// remove an element from the end of the array
console.log(myArray.pop());

  
// unshift - add an element in the front of the array
const newLength = myArray.unshift(42);

console.log(myArray);
console.log(newLength);

  
// shift - remove the element from the front of the array
const removedElement = myArray.shift();

  
console.log(myArray);
console.log(removedElement);


// delete - don't use this because it doesn't shift the index and make position value undefined

//delete myArray[1];
console.log(myArray);
console.log(myArray[1]);

  
// For removing and delete the element from the array use splice method.
// Delete an element at index 1
myArray.splice(1, 1);
console.log(myArray);

  
// Replace the value of an element at index 1
myArray.splice(1, 1, true);
console.log(myArray);

  
// Insert the value of an element at index 1
myArray.splice(1, 0, 54);
console.log(myArray);

  
myArray.splice(3, 0, 987);
console.log(myArray);

  

// slice method
const myArray2 = ["A", "B", "C", "D", "E", "F"];

const newArray = myArray2.slice(2,5);
console.log(newArray);


// reverse method
myArray2.reverse();
console.log(myArray2);


// join method
const newString = myArray2.join();
console.log(newString);


// split method
const splittedArray = newString.split(",");
console.log(splittedArray);


// concat
const myArrayA = ["A", "B" , "C"];
const myArrayB = ["D", "E" , "F"];


const concatArray = myArrayA.concat(myArrayB);
console.log(concatArray);


// spread operator
let spreadArray = [...myArrayA,...myArrayB];
console.log(spreadArray);

  

// Multidimensional Array 2D
const equipShelfA = ["baseball", "football", "volleyball"];
const equipShelfB = ["basketball", "golf balls", "tennis balls"];
const clothesShelfA = ["tank tops", "t-shirts", "jerseys"];
const clothesShelfB = ["sweat tops", "sweat pants", "hoodies"];
const equipDept = [equipShelfA, equipShelfB];
const clothesDept = [clothesShelfA, clothesShelfB];

console.log(equipDept[0][1]);
console.log(clothesDept[1][0]);


// 3D array
const sportsStore = [equipDept, clothesDept];
console.log(sportsStore[0][0][1]);
console.log(sportsStore[1][1][0]);
```

#### Iterating an array

To iterate over an array, you could use the for-of loop.

```js
const numbers = [1, 2, 3, 4, 5];

for (const number of numbers)
    console.log(number);
```

There is another built in method in the array class, known as .forEach(), so you pass a callback function as the argument for .forEach().

```js
numbers.forEach((number) => {
    console.log(number);
});
```

Since the arrow function's code block is just one line we can put everything on one line to simplify it.
```js
numbers.forEach(number => console.log(number));
```

The argument for the .forEach() method also accepts an optional second parameter,
which is the index of the corresponding element.

```js
numbers.forEach((number, index) => console.log(`At index, ${index}: ${number}`));
```

#### Joining Arrays

To transform an array into a string, the join() method can be utilized.

Can convert the array into a string, where you specify the separate as the argument which will be placed in between the elements.

```js
const numbers = [1, 2, 3, 4, 5];

const joinedNumbers = numbers.join(', '); 
console.log(joinedNumbers); // 1, 2, 3, 4, 5
```

Likewise, the .split() method is available for strings, this is done to convert a string into an array.
This will not alter the original string, rather it will return an array.

```js
const courseName = 'JavaScript for Beginners';
const parts = courseName.split(' ');
console.log(parts);

```

So an example which shows how this could be useful, consider the term, URL (Uniform Resource Locator) slug. This refers to having a descriptive path in your URL.
URL slugs are separated by a hyphen.

Utilize method chaining
```js
const urlSlug = courseName
    .toLowerCase()
    .split(' ')
    .join('-'); // javascript-for-beginners
```

#### Sorting an array

In programming, sorting is a common operation. JavaScript arrays come with a built-in sort method, that allows you to easily sort array elements.

By default when you call sort on an array containing strings or numbers, it sorts the elements in ascending order.

```js
let characters = ['c', 'd', 'b', 'a'];
characters.sort();
console.log(characters); // [ 'a', 'b', 'c', 'd' ]
```

Another useful method in the array data structure is .reverse(). This can be used to reverse the order of the elements.

```js
characters.reverse();
console.log(characters);
```

When you have an array of objects, you need to provide a callback function to the sort method.
This function defines the sorting logic based on the properties of the object in the array.

So since we are sorting reference types, we need to pass a callback function for the .sort() method. This callback function will accept two parameters which will represent two elements in this case. For the callback function, consider the two parameters, obj1 and obj2.

If the result is a negative number, then obj1 would come before obj2 in the final sorted array.
If the result is 0 then obj1 is equal to obj2 and no need to swap any elements.
If the result is a positive number, then obj1 would come after obj2 in the final sorted array.

NOTE : Characters are represented internally as numbers in computers based on their ASCII values. So capital letters are considered less than lowercase letters. Therefore, we want to make the strings all the same case. (case insensitive).

```js
let employees = [
    { id: 1, name: 'Jen' },
    { id: 2, name: 'Steven' },
    { id: 3, name: 'Andrew' },
    { id: 4, name: 'Terry' },
];

employees.sort((a, b) => {
    const lowercaseA = a.name.toLowerCase();
    const lowercaseB = b.name.toLowerCase();

    if (lowercaseA < lowercaseB) return -1;
    if (lowercaseA > lowercaseB) return 1;
    return 0;
});

console.log(employees);
/*
[
  { id: 3, name: 'Andrew' },
  { id: 1, name: 'Jen' },
  { id: 2, name: 'Steven' },
  { id: 4, name: 'Terry' }
]
*/
```

#### Testing elements of an array.

In JavaScript arrays come equipped with several powerful methods that allow us to process and evaluate the data within them efficiently. Two such methods are .every() and .some() both of which are used to test elements in an array against a condition.

.every() tests whether all elements of an array pass the test of the provided function. 
It returns true if every element in the array satisfies the condition and false otherwise.

so .every() can accept up to three parameters
1st param: the current element of the array
2nd param (optional): the index of where the element is stored
3rd param (optional): the original array

```js
let numbers = [2, 4, 6, 8, 10];

const areAllEven = numbers.every(number => number % 2 === 0);
console.log(`areAllEven: ${areAllEven}`); // true

```

.some() checks if at least one element in the array passes the test implemented by the provided function.

1st param: the current element of the array
2nd param (optional): the index of where the element is stored
3rd param (optional): the original array

```js
numbers = [1, 3, 5, 7, 8, 9];

const hasOneEvenNumber = numbers.some(number => number % 2 === 0); // true - 8 is even.
```

#### Filtering an Array

One of the most versatile built in methods for arrays in JavaScript is the filter method.

This method is designed to go through an array and extract elements  that meet a specific condition. Returning a new array comprised of only those elements.

It's useful for creating subsets of an array that match certain criteria.

So you pass in a callback predicate function as an argument. Once again, predicate functions are functions that return a boolean value. (true or false).

```js
const numbers = [1, 2, 3, 4, 5, 6];

const evenNumbers = numbers.filter(number => number % 2 === 0);
console.log(evenNumbers);

const employees = [
    { id: 1, name: 'Alice', role: 'Developer' },
    { id: 2, name: 'Bob', role: 'Designer' },
    { id: 3, name: 'Charlie', role: 'Manager' },
    { id: 4, name: 'Danielle', role: 'Developer' },
];

const developers = employees.filter(employee => employee.role === 'Developer');
console.log(developers);
```

#### Mapping an Array

The .map() method is a cornerstone of array manipulation. It's a powerful way to process and transform array elements.

It operates on each element of an array, applying a function that you specify, and returns a new array composed of the results.

So it allows you to transform data without altering the original array.

```js
const numbers = [2, 4, 6, 8, 10];
const squaredNumbers = numbers.map(num => num * num);
console.log('squaredNumbers ', squaredNumbers);

const characters = ['a', 'b', 'c', 'd'];
const upperCaseCharacters = characters.map(char => char.toUpperCase());
console.log('upperCaseCharacters', upperCaseCharacters);

const employees = [
    { id: 1, name: 'Alice', email: 'AliCe@gmail.com' },
    { id: 2, name: 'Steven', email: 'STeVen@gmail.com' },
    { id: 3, name: 'Joe', email: 'Joe@gmail.com' },
];

const updatedEmployees = employees.map(employee => ({
    ...employee,
    email: employee.email.toLowerCase()
}));

/*
[
  { id: 1, name: 'Alice', email: 'alice@gmail.com' },
  { id: 2, name: 'Steven', email: 'steven@gmail.com' },
  { id: 3, name: 'Joe', email: 'joe@gmail.com' }
]
*/
```

#### Reducing an array

JavaScript has a very powerful array method called reduce. It can transform an array into just about anything, such as a number, string, object, or another array.

This is how you would traditionally calculate the sum of an array of numbers.

```js
const numbers = [1, 10, 5, 14];
let sum = 0;

for (const number of numbers)
    sum += number;

console.log(`Total sum: ${sum}`); // 40
```

There's a cleaner way to accomplish this with the sum method.

1st argument is a callback function, this should take in two parameters for the accumulatorValue and the currentValue

2nd argument is the initial value for the accumulator.

```js
let sum = 0;
sum = numbers.reduce((accumulator, currentValue) => {
console.log(`accumulator = ${accumulator}`);
console.log(`currentValue = ${currentValue}`);
return accumulator + currentValue;
} , 0);

console.log(sum); // 40
/*
accumulator = 0
currentValue = 1
accumulator = 1
currentValue = 10
accumulator = 11
currentValue = 5
accumulator = 16
currentValue = 14
*/
```

### Adding Elements

One of the most common operations when dealing with arrays is adding elements.

Using the const keyword, we can't reassign this variable to any other value, but we can modify the array (the object) that it references.

```js
const numbers = [5, 4, 3, 2, 1];
```

There are 3 ways to add an element to an array.

1. Add to the beginning with .unshift()

```js
numbers.unshift(14); //[14, 5, 4, 3, 2, 1]
numbers.unshift(17, 19); //[17, 19, 14, 5, 4, 3, 2, 1]
```

2. Add to the middle with .splice().

1st arg is the index position to start from, 
2nd arg is the number of elements to delete
then can specify one or more elements to add from that index position specified from the 1st arg

```js
numbers.splice(1, 0, 18); // [ 5, 18, 4, 3, 2, 1 ]
numbers.splice(1, 0, 25, 24); // [ 5, 25, 24, 18, 4,  3,  2,  1 ]
```

3. Add to the end with .push() 

```js
numbers.push(7); // [5, 4, 3, 2, 1, 7]
numbers.push(10, 11, 12); // [5, 4, 3, 2, 1, 10, 11, 12]
```

#### Finding Primitive Elements:

Arrays can store both primitive data types or reference data types (objects).
There are three main methods we can use to check if an array contains a primitive value.

```js
const numbers = [5, 4, 3, 2, 1, 3];
```

1. .indexOf()

```js
const indexOfThree = numbers.indexOf(3);
console.log('indexOfThree', indexOfThree); // outputs 2, since the element 3, is at the index of 2
console.log( numbers[indexOfThree] );
```

2. .lastIndexOf() - searches from right to left

```js
const lastIndexOfThree = numbers.lastIndexOf(3);
console.log('lastIndexOfThree', lastIndexOfThree); // outputs 5 since the element 3's last occurence is at the index of 5
```

If an element is not found, then the index will be -1
if you try accessing the array at the position of -1, then it will be undefined

So whenever we do .indexOf() or .lastIndexOf()
we would usually have a conditional statement to ensure the index position is not -1

```js
const indexOfTen = numbers.indexOf(10);
console.log(indexOfTen); // -1

console.log(numbers[-1]); // undefined

if (indexOfTen === -1) {
    console.log('10 is not found in the array');
}
```

3. .includes()

If we are not utilizing that index value in order to access or change that element, then we can just use the includes method which will return true or false.

```js
if (numbers.includes(10)) {
    console.log('10 is found in the array');
}

if (!numbers.includes(10)) {
    console.log('10 is not found in the array');
}
```

#### Finding Reference Types in Array.

The employees object stores 3 objects/reference types.

When dealing with reference types, we can use the indexOf(), lastIndexOf(), or includes() methods built into arrays in order to find an element.

As if you compare two different objects, even if they have the same property values,
they won't be equal and it would be comparing their memory addresses, which differ.

Rather we would have to use .find(), .findIndex(), and pass in a predicate callback function
as the argument.

```js
const employees = [
    {
        id: 1,
        name: 'Jim',
    },
    {
        id: 2,
        name: 'Michael Scott',
    },
    {
        id: 3,
        name: 'Pam',
    },
];
```

We pass a callback function which in this case is a predicate function.

A predicate function is one which returns a boolean value (true or false).

So .find() returns the first element that matches

```js
let employee = employees.find(function(e) {
    return e.name === 'Jim'
});
```

#### Arrow Functions

Previously we used the function syntax for the callback function for the find() method.

We pass a callback function which in this case is a predicate function. A predicate function is one which returns a boolean value (true or false).

Here we use the arrow function syntax.

So .find() returns the first element that matches.

```js
const employees = [
    {
        id: 1,
        name: 'Jim',
    },
    {
        id: 2,
        name: 'Michael Scott',
    },
    {
        id: 3,
        name: 'Pam',
    },
];

let employee = employees.find(e => e.name === 'Jim');
```

When you have an arrow function that has just one statement in the code block, then you can put everything on one line.

```js
const add = (num1, num2) => {
    return num1 + num2;
}

const add = (num1, num2) => num1 + num2;
```

#### Removing Elements

To remove elements: 
1) .pop() to remove from the end.
2) .splice() to remove from the middle.
3) .shift() to remove from the beginning.

```js
const numbers = [1, 2, 3, 4, 5];

// NOTE: \n within a string stands for newline,

const lastElement = numbers.pop();
console.log(`lastElement: ${lastElement}\n`); // lastElement: 5

const firstElement = numbers.shift();
console.log(`firstElement: ${firstElement}`); // firstElement: 1

// 1st arg is the index to start from
// 2nd arg is the number of elements to remove
const middleElement = numbers.splice(1, 1);
console.log(`middleElement: ${middleElement}`); // middleElement: 3
```

#### Emptying an array

If you want to clear all the elements of an array, there are different ways that you can do that.

```js
let numbers = [1, 2, 3, 4, 5];

// If the numbers array was large, then this would be inefficient.
while (numbers.length > 0)
    numbers.pop();
```

Could set the length property to 0.

```js
numbers = [1, 2, 3, 4, 5];
numbers.length = 0; // we could assign it to 0, clearing/emptying the array
console.log(numbers); // []
```

Could use the .splice() method.

```js
numbers = [1, 2, 3, 4, 5];

const deletedNumbers = numbers.splice(0, numbers.length);
console.log(`deletedNumbers: ${deletedNumbers}`); // deletedNumbers: 1,2,3,4,5
```

Could reassign the array to an empty array, the previous array would be garbage collected.

```js
numbers = [1, 2, 3, 4, 5];
numbers = [];
```

#### Combining and Slicing

Let's say we have two arrays and we want to combine it into one array.

```js
const exampleNumbersA = [1, 2, 3];
const exampleNumbersB = [4, 5, 6];

// Can combine these arrays using the concat method.
const combinedArray = exampleNumbersA.concat(exampleNumbersB);
console.log('combinedArray', combinedArray); //combinedArray [ 1, 2, 3, 4, 5, 6 ]
```

slice(startIndex, endIndex) where the endIndex is exclusive (not included)
The slice method will not affect the original array.

If you doing this will primitive values, then the elements will be passed by copy.
However if you're dealing with objects, then the elements will be passed by reference.

```js
const firstSlice = combinedArray.slice(0, 4);
console.log('firstSlice', firstSlice); // firstSlice [ 1, 2, 3, 4 ]
```

#### Spread Operator

There is another way to combine two arrays, which is to utilize the spread operator.

So the spread operator can be used to copy the properties of an object or the elements of an array.

```js

const exampleNumbersA = [1, 2, 3];
const exampleNumbersB = [4, 5, 6];

// So rather than doing
// let combined = exampleNumbersA.concat(exampleNumbersB);
// Below is the more commonly used syntax.

let combined = [...exampleNumbersA, ...exampleNumbersB];
console.log(combined); // [ 1, 2, 3, 4, 5, 6 ]

combined = [...exampleNumbersA, 9, ...exampleNumbersB, 10];
```

Arrays are objects, meaning that they are passed by reference.

```js
let a = [1, 2];
let b = a;
```

So we could also use the spread operator in order to make a copy of an array.

```js
b = [...a];
```

