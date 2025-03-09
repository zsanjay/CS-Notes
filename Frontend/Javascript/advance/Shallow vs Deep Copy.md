#### Foundational Knowledge for Writing Pure Functions

#### Javascript Data Types

#### Primitive vs Structural

##### Primitive:

1) undefined
2) Boolean
3) Number
4) String
5) BigInt
6) Symbol

Structural:

1) Object: (new) Object, Array, Map, Set, WeakMap, Date
2) Function

#### Value vs Reference

##### Primitive pass values:

```js
let x = 2;
let y = x;
y += 1;

console.log(y); // 3
console.log(x); // 2

// Structural types use references:

let xArray = [1, 2, 3];
let yArray = xArray;
yArray.push(4);

console.log(yArray); // [1, 2, 3, 4]
console.log(xArray); // [1, 2, 3, 4]

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
```

##### Pure Function require you to avoid mutating the data.

#### Impure function 

- It mutates the original array.
- It is considered to be a side-effect.
#### const

Notice: "const" does not make the array immutable, it stops you to reassign a value.

```js
const addToScoreHistory = (array, score) => {
	array.push(score);
	return array;
}

const scoreArray = [44, 23, 12];
console.log(addToScoreHistory(scoreArray, 14)); // [44, 23, 12, 14]
```
#### Shallow copy vs Deep copy (clones).

#### Shallow copy

1. With the spread operator
2. With Object.assign()


```js
// 1. With the spread operator
const zArray = [...yArray , 10];

console.log(zArray); // [1, 2, 3, 10]
console.log(yArray === zArray); // false

  
// 2. With Object.assign()
const tArray = Object.assign([], zArray);

console.log(tArray); // [1, 2, 3, 10]
console.log(tArray === zArray); // false

tArray.push(11);

console.log(zArray); // [1, 2, 3, 10]
console.log(tArray); // [1, 2, 3, 10, 11]
```

But if there are nested arrays or objects...

```js
yArray.push([8, 9, 10]);

console.log(yArray); // [1, 2, 3,[8, 9, 10]]

const vArray = [...yArray];

console.log(vArray); // [1, 2, 3,[8, 9, 10]]

vArray[vArray.length - 1].push(11); // [1, 2, 3,[8, 9, 10, 11]]

console.log(vArray[vArray.length - 1]); // [8, 9, 10, 11]
console.log(yArray[yArray.length - 1]); // [8, 9, 10, 11]

```

Nested structural data types still share a reference when we use a shallow copy!
Note: Array.from() and slice() create shallow copies, too.

When it comes to objects, what about Object.freeze() ?

```js
const scoreObj = {
	"first" : 44,
	"second" : 12,
	"third" : { "a" : 1, "b": 2}
}

Object.freeze(scoreObj);
scoreObj.third.a = 8;

console.log(scoreObj); // { first: 44, second: 12, third: { a: 8, b: 2 } }
```

Still mutates - it is a shallow freeze.

##### Deep Copy

Several libraries like lodash, Ramda, and others have this feature built-in. Here is a one line Vanilla JS solution, but it does not work with Dates, functions, undefined, Infinity, RegExps, Maps, Sets, Blobs, FileLists, ImageDatas, and other complex data types.

```js
const newScoreObj = JSON.parse(JSON.stringify(scoreObj));

console.log(newScoreObj); // { first: 44, second: 12, third: { a: 1, b: 2 } }
console.log(newScoreObj === scoreObj); // false

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
const scoreArray = [44, 23, 12, 14];
const newScoreArray = deepClone(scoreArray);

console.log(newScoreArray); // [ 44, 23, 12, 14 ]
console.log(newScoreArray === scoreArray); // false


// Deep cloning object
const myScoreObj = deepClone(scoreObj);

console.log(myScoreObj); // { first: 44, second: 12, third: { a: 1, b: 2 } }
console.log(myScoreObj === scoreObj); // false


// Now we can make a pure function
const pureAddToScoreHistory = (array, score, cloneFunc) => {
	const newArray = cloneFunc(array);
	newArray.push(score);
	return newArray;
}

const pureScoreHistory = pureAddToScoreHistory(scoreArray, 18, deepClone);

console.log(pureScoreHistory); // [ 44, 23, 12, 14, 18]
console.log(scoreArray); // [ 44, 23, 12, 14]
```

### References

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures

https://dev.to/glebec/four-ways-to-immutability-in-javascript-3b3l

https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript

https://www.youtube.com/watch?v=4Ej0LwjCDZQ&list=PL0Zuz27SZ-6N3bG4YZhkrCL3ZmDcLTuGd&index=5