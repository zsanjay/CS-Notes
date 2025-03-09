
Programming is powerful as it enables us to execute different code based on conditions.
This is what enables us to provide dynamic and personalized applications to end users.

The fundamental programming concept that enables this is conditional statements.
###### More specifically, if-else statements.

```js
let priceOfChocolate = 1.99;
let hasAmountInCash = 5;

const canBuyChocolate = hasAmountInCash >= priceOfChocolate;

console.log('canBuyChocolate', canBuyChocolate);
console.log(typeof canBuyChocolate);

if (hasAmountInCash >= priceOfChocolate) {
    console.log('Enjoy your purchase');
} else {
    console.log('Sorry you do not have enough');
}

let hour = 10;

if (hour > 6 && hour <= 12) {
    console.log('Serving breakfast');
} else if (hour > 12 && hour <= 14) {
    console.log('Serving lunch');
} else {
    console.log('Serving dinner');
}
```

Implement a function that accepts two numbers and returns the maximum number.

```js
function max(num1, num2) {
    return num1 >= num2 ? num1 : num2;
}
```

Implement a function to accept a number.

Then return "FizzBuzz" if divisible by 3 and 5.
Or return "Fizz" if only divisible by 3.
Or return "Buzz" if only divisible by 5.
Or return the original number if not divisible by 3 or 5

```js
function fizzBuzz(num) {
    if (typeof num !== 'number') return num;

    if (num % 3 === 0 && num % 5 === 0)
        return 'FizzBuzz';
    else if (num % 3 === 0)
        return 'Fizz';
    else if (num % 5 === 0)
        return 'Buzz';
    else
        return num;
}
```

Write a function to display even and odd numbers.

```js
let number = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const evenNums = number.filter(x => x % 2 === 0);
const oddNums = number.filter(x => x % 2 !== 0);

console.log(evenNums);
console.log(oddNums);
```

##### Switch Case Statement

Switch-case statements can also be used for control flow.

The difference between if-else statements and switch-case statements is that switch-case statements are only used for equality comparisons.

```js
let job = 'Software Developer';

if (job === 'Software Developer') {
    console.log('Writes code');
} else if (job === 'Designer') {
    console.log('Makes user interface documents');
} else if (job === 'Cloud Engineer') {
    console.log('Manages and deploys cloud resources');
} else {
    console.log('Works directly with customers');
}
```

Since we doing equality comparisons, this can be hard to read and repetitive. So in this case, we could use the switch-case statements.

```js
switch (job) {
    case 'Software Developer':
        console.log('Writes code');
        break;
    case 'Designer':
        console.log('Makes user interface documents');
        break;
    case 'Cloud Engineer':
        console.log('Manages and deploys cloud resources');
        break;
    default:
        console.log('Works directly with customers');
}
```

#### For Loops

```js
let numbers = [1, 2, 3, 4, 5, 6, 7];

// To output all of the elements in the array.

let idx = 0;
let lengthOfArray = numbers.length;

console.log( numbers[idx++] );
console.log( numbers[idx++] );
console.log( numbers[idx++] );
console.log( numbers[idx++] );
console.log( numbers[idx++] );
console.log( numbers[idx++] );
console.log( numbers[idx] );

// You can use the for loop to efficiently iterate through an array
for (let idx = 0; idx < numbers.length; idx++) {
    console.log(numbers[idx]);
}
```

#### While Loops

We use for loops when we know the exact number of times that we want the loop to execute.

```js
let numbers = [1, 2, 3, 4, 5, 6, 7];

for (let idx = 0; idx < numbers.length; idx++) {
    console.log(numbers[idx]);
}
```

There's another way to perform loops, which is the while-loop.

You would use the while-loop when you know which condition must be true, to perform the loop, but not the exact number of times you want the loop to be performed.

```js
let idx = 0;
while (idx < numbers.length) {
    console.log(numbers[idx]);
    idx++;
}

let sum = 0;
while (true) {
    console.log('Loop');
    sum++;

    if (sum === 10)
        break;
}
```

#### do while Loops

Another loop in JavaScript is the do-while loop.
This differs from the traditional while-loop as it will execute the statements within the code block, and then evaluate the condition after.

This means that a do-while loop is guaranteed to execute at least once.

```js
let i = 0;
do {
    console.log(i);
    i++;
} while (i < 10);
```

#### Infinite Loops

Infinite loops will cause your program to crash.

You want to ensure within your loops that you are progressively getting closer to your condition being false as to terminate the loop.

```js
while(true) {
	console.log("Infinite Loop");
}
```

#### For in Loop

We typically use for-loops to iterate over an array.

We also have another loop known as the for-in loop which is used to iterate over the keys of a JavaScript object.

A JavaScript object is a data type that allows you to store key-value pairs.

For in loop returns key or index.

```js
const course = {
    name: 'JavaScript for Beginners',
    duration: 3,
    setions: 7
};

console.log(course.name);
console.log(course['duration']);
console.log(course.sections);

// We could also iterate over the keys with the for-in loop.
for (const key in course) {
    console.log(course[key]);
}

let numbers = [1, 2, 3, 4, 5, 6, 7];

for(const index in number) {
	console.log(numbers[index]);
}
```

#### For of Loop

In the case when we are simply looping over the elements of an array. If we aren't utilizing the index variable, then we have a cleaner syntax. For of returns value.

```js
let numbers = [1, 2, 3, 4, 5, 6, 7];

for (const number of numbers) {
    console.log(number);
}
```

#### break and continue

In loops, there may be specific conditions where you will want to terminate the loop.
This is where you would use the break statement.

```js
//break statement in the for loop
for (let i = 0; i < 10; i++) {
    if (i === 5)
        break;

    console.log(i);
}

// break statement in the while loop
let i = 0;
while (i < 10) { 
    if (i === 5)
        break;

    console.log(i);
    i++;
}

// break statement in the do-while loop
let doWhileIdx = 0;
do {
    if (doWhileIdx === 5)
        break;

    console.log(doWhileIdx);

    doWhileIdx++;
} while (doWhileIdx < 10);

// break statement in the for-in loop
let object = { a: 1, b: 2, c: 3 };
for (const key in object) {
    if (key === 'b') break;

    console.log(object[key]);
}

// break statement in the for-of loop
const array = [1, 2, 3, 4, 5];
for (let element of array) {
    if (element === 3) break;

    console.log(array[element]);
}
```

There may also be times when you want to skip to the next iteration of the loop,
which is where you would use the continue statement.

Now let us consider the continue keyword.

```js
// continue statement in the for loop
for (let i = 0; i < 10; i++) {
    if (i % 2 === 0)
        continue;

    console.log(i);
}

// continue statement in the while loop
let whileLoopIdx = 0;
while (whileLoopIdx < 10) {
    if (whileLoopIdx % 2 === 0)
        continue;

    console.log(whileLoopIdx);
    whileLoopIdx++;
}

// continue statement in the do-while loop
doWhileIdx = 0;
do {
    if (doWhileIdx % 2 === 0)
        continue;

    console.log(doWhileIdx);
    doWhileIdx++;
} while (doWhileIdx < 10);

// continue statement in the for-in loop
object = { a: 1, b: 2, c: 3, d: 4 };

for (const key in object) {
    if (object[key] % 2 === 0) continue;

    console.log(object[key]);
}

// continue statement in the for-of loop
for (let element of array) {
    if (element % 2 === 0) continue;

    console.log(array[element]);
}
```

