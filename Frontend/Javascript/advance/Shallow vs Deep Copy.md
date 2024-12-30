```js
// Foundational Knowledge for Writing Pure Functions

// Javascript Data Types

// Primitive vs Structural

/* Primitive:

1) undefined

2) Boolean

3) Number

4) String

5) BigInt

6) Symbol

*/

  

/* Structural:

1) Object: (new) Object, Array, Map, Set, WeakMap, Date

2) Function

*/

  

// Value vs Reference

// Primitive pass values:

  

let x = 2;

let y = x;

y += 1;

console.log(y);

console.log(x);

  

// Structural types use references:

let xArray = [1, 2, 3];

let yArray = xArray;

yArray.push(4);

console.log(yArray);

console.log(xArray);

  

// Mutable vs Immutable

  

// Primitives are immutable

let myName = "Dave";

myName[0] = "W"; // we can't change this

console.log(myName);

  

// Reassignment is not the same as mutable

myName = "David";

console.log(myName);

  

// Structures contain mutable data

yArray[0] = 9;

console.log(yArray);

console.log(xArray);

  
  

// Pure Function require you to avoid mutating the data.

  

// Impure function that mutates the data

const addToScoreHistory = (array, score) => {

array.push(score);

return array;

}

  

const scoreArray = [44, 23, 12];

console.log(addToScoreHistory(scoreArray, 14));

  

// This mutates the original array.

// This is considered to be a side-effect.

  

// Notice: "const" does not make the array immutable,

// it stops you to reassign a value.

  

// Shallow copy vs Deep copy (clones)

  

// Shallow copy

// 1. With the spread operator

const zArray = [...yArray , 10];

console.log(zArray);

  

console.log(xArray === yArray);

console.log(yArray === zArray);

  

// 2. With Object.assign()

const tArray = Object.assign([], zArray);

console.log(tArray);

console.log(tArray === zArray);

tArray.push(11);

console.log(zArray);

console.log(tArray);

  

// But if there are nested arrays or objects...

yArray.push([8, 9, 10]);

console.log(yArray);

const vArray = [...yArray];

console.log(vArray);

vArray[vArray.length - 1].push(11);

console.log(vArray[vArray.length - 1]);

console.log(yArray[yArray.length - 1]);

  

// nested structural data types still share a reference when we use a shallow copy!

  

// Note: Array.from() and slice() create shallow copies, too.

  

// When it comes to objects, what about...

// ...Object.freeze() ??

  

const scoreObj = {

"first" : 44,

"second" : 12,

"third" : { "a" : 1, "b": 2}

}

  

Object.freeze(scoreObj);

scoreObj.third.a = 8;

console.log(scoreObj);

// still mutates - it is a shallow freeze

  

// How do we avoid these mutations?

  

// Deep copy is needed to avoid this

  

// Several libraries like lodash, Ramda, and others

// have this feature built-in

  

/* Here is a one line Vanilla JS solution,

but it does not work with Dates, functions, undefined, Infinity, RegExps,

Maps, Sets, Blobs, FileLists, ImageDatas, and other complex data types

*/

  

const newScoreObj = JSON.parse(JSON.stringify(scoreObj));

console.log(newScoreObj);

console.log(newScoreObj === scoreObj);

  

console.clear();

// Instead of using a library, here is a Vanilla JS function

  

const deepClone = (obj) => {

  

if(typeof obj !== "object" || obj === null) return obj;

  

// Create an array or object to hold the values

const newObject = Array.isArray(obj) ? [] : {};

  

for(let key in obj) {

const value = obj[key];

// recursive call for nested objects & arrays

newObject[key] = deepClone(value);

}

return newObject;

}

  

// Deep cloning array

const newScoreArray = deepClone(scoreArray);

console.log(newScoreArray);

console.log(newScoreArray === scoreArray);

  

// Deep cloning object

const myScoreObj = deepClone(scoreObj);

console.log(myScoreObj);

console.log(myScoreObj === scoreObj);

  

// Now we can make a pure function

const pureAddToScoreHistory = (array, score, cloneFunc) => {

const newArray = cloneFunc(array);

newArray.push(score);

return newArray;

}

  

const pureScoreHistory = pureAddToScoreHistory(scoreArray, 18, deepClone);

  

console.log(pureScoreHistory);

console.log(scoreArray);
```

### References

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures

https://dev.to/glebec/four-ways-to-immutability-in-javascript-3b3l

https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript

https://www.youtube.com/watch?v=4Ej0LwjCDZQ&list=PL0Zuz27SZ-6N3bG4YZhkrCL3ZmDcLTuGd&index=5