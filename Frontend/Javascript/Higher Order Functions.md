
A higher order function is a function that does at least one of the following:

1. Takes one or more functions as a parameter.
2. Returns a function as the result.

```js
// Higher Order Functions
import { posts } from "../data/post.js";

posts.forEach((post) => { console.log(post); });
console.clear();


const filteredPosts = posts.filter((post) => post.userId === 5);
console.log(filteredPosts);


const mappedPosts = filteredPosts.map(post =>  post.id * 10);
console.clear();
console.log(mappedPosts);


const reducedPostsValue = mappedPosts.reduce((sum, post) => {
	return sum + post;
});

console.log(reducedPostsValue);
```

## What is a Higher Order Function?

A higher order function is a function that takes one or more functions as arguments, or returns a function as its result. 

There are several different types of higher order functions like map and reduce. We will discuss these later in this tutorial. But before that let's first dive deep into what higher order functions are.

```js
// Callback function, passed as a parameter in the higher order function
function callbackFunction(){
    console.log('I am  a callback function');
}

// higher order function
function higherOrderFunction(func){
    console.log('I am higher order function')
    func()
}

higherOrderFunction(callbackFunction);
```

In the above code `higherOrderFunction()` is an HOF because we are passing a callback function as a parameter to it. 

The above example is quite simple isn't it? So let's expand it further and see how you can use HOFs to write more concise and modular code.

### How Higher Order Functions Work?

So, suppose I want you to write a function that calculates the area and diameter of a circle. As a beginner, the first solution that comes to our mind is to write each separate function to calculate area or diameter.

```js
const radius = [1, 2, 3];
```

```js
// function to calculate area of the circle
const calculateArea =  function (radius) {
    const output = [];
    for(let i = 0; i< radius.length; i++){
        output.push(Math.PI * radius[i] * radius[i]);
    }
    return output;
}
```

```js
// function to calculate diameter of the circle
const calculateDiameter =  function (radius) {
    const output = [];
    for(let i = 0; i< radius.length; i++){
        output.push(2 * radius[i]);
    }
    return output;
}
```

```js
console.log(calculateArea(radius));
console.log(calculateDiameter(radius))
```

But have you noticed the problem with the above code? Aren't we writing almost the same function again and again with slightly different logic? Also, the functions we have written aren't reusable, are they?

So, let's see how we can write the same code using HOFs:

```js
const radius = [1, 2, 3];
```

```js
// logic to clculate area
const area = function(radius){
    return Math.PI * radius * radius;
}
```

```js
// logic to calculate diameter
const diameter = function(radius){
    return 2 * radius;
}
```

```js
// a reusable function to calculate area, diameter, etc
const calculate = function(radius, logic){ 
    const output = [];
    for(let i = 0; i < radius.length; i++){
        output.push(logic(radius[i]))
    }
    return output;
}
```

```js
console.log(calculate(radius, area));
console.log(calculate(radius, diameter));
```

Now, as you can see in the above code, we have only written a single function `calculate()` to calculate the area and diameter of the circle. We only need to write the logic and pass it to `calculate()` and the function will do the job.

The code that we have written using HOFs is concise and modular. Each function is doing its own job and we are not repeating anything here. 

Suppose in the future if we want to write a program to calculate the circumference of the circle. We can simply write the logic to calculate the circumference and pass it to the `calculate()` function.

```js
const circumeference = function(radius){
    return 2 * Math.PI * radius;
}
```

```js
console.log(calculate(radius, circumeference));
```

## How to Use Higher Order Functions

You can use higher order functions in a variety of ways. 

When working with arrays, you can use the `map()`, `reduce()`, `filter()`, and `sort()` functions to manipulate and transform data in an array. 

When working with objects, you can use the `Object.entries()` function to create a new array from an object. 

When working with functions, you can use the `compose()` function to create complex functions from simpler ones. 

## How to Use Some Important Higher Order Functions?

There are various built in HOFs, and some of the most common ones are map(), filter() and reduce(). So let's understand each one of these in detail.
### **How to use `map()` in JavaScript**?

The `map()` function takes an array of values and applies a transformation to each value in the array. It does not mutate the original array. It is often used to transform an array of data into a new array with a different structure.

Let's understand with the help of examples.

**Example 1**: Suppose we want to add 10 to every element in a array. We can use the `map()` method to map over every element in the array to add 10 to it.

```js
const arr = [1, 2, 3, 4, 5];
const output = arr.map((num) => num += 10)
console.log(arr); // [1, 2, 3, 4, 5]
console.log(output); // [11, 12, 13, 14, 15]
```

In the above example, `arr` is an array with five elements: 1, 2, 3, 4, and 5. `map` is a method that we use to apply a function to each element in an array, and it returns a new array with the modified elements.

The callback function that is being passed to `map` uses the arrow function syntax, and it takes a single argument `num`. This function adds 10 to `num`(every element in the array) and returns the result.

**Example 2**: Here we have an array of users. Suppose we only want their first and last name. We can simply use the `map()` method to extract it from the `users` array.

```js
const users = [
    {firstName: 'John', lastName: 'Doe', age: 25},
    {firstName: 'Jane', lastName: 'Doe', age: 30},
    {firstName: 'Jack', lastName: 'Doe', age: 35},
    {firstName: 'Jill', lastName: 'Doe', age: 40},
    {firstName: 'Joe', lastName: 'Doe', age: 45},
]

const result = users.map((user) => user.firstName + ' ' + user.lastName)
console.log(result); // ['John Doe', 'Jane Doe', 'Jack Doe', 'Jill Doe', 'Joe Doe']
```

In the above code, `users` is an array of objects representing users. Each object has three properties: `firstName`, `lastName`, and `age`.

We are mapping over each user using the `map()` method to extract the properties `firstName` and `lastName`.

The callback function takes a single argument `user` which represents an element in the `users` array (an object). 

The function concatenates the `firstName` and `lastName` properties of the `user` object, and returns the result.

### **How to Use `filter()` in JavaScript**

The `filter()` function takes an array and returns a new array with only the values that meet certain criteria. It also does not mutate the original array. It is often used to select a subset of data from an array based on certain criteria.

**Example 1**: You can use `filter()` to return only the odd numbers from an array of numbers.

```js
const arr = [1, 2, 3, 4, 5];
const oddNums = arr.filter((num) => num % 2) // filter out odd numbers
const evenNums = arr.filter((num) => !(num % 2)) // filter out odd numbers
console.log(arr); // [1, 2, 3, 4, 5]
console.log(output); // [1, 3, 5]
```

In the above code, `arr` is an array with five elements: 1, 2, 3, 4, and 5. `filter` is a method that is used to create a new array with elements that pass a test specified in a provided callback function.

This callback function checks if `num` is odd by checking if it is not divisible by 2 (`num % 2`). If `num` is not divisible by 2, the function returns `true`, otherwise it returns `false`.

When `filter` is called on `arr`, it applies this function to each element in the array, creating a new array with only the elements that returned `true`or passed the specified condition when passed to the function. The original `arr` remains unchanged and returns the result.

**Example 2**: You can use `filter()` to return only the users having age greater than 30 in an array.

```js
const users = [
    {firstName: 'John', lastName: 'Doe', age: 25},
    {firstName: 'Jane', lastName: 'Doe', age: 30},
    {firstName: 'Jack', lastName: 'Doe', age: 35},
    {firstName: 'Jill', lastName: 'Doe', age: 40},
    {firstName: 'Joe', lastName: 'Doe', age: 45},
]

// Find the users with age greater than 30
const output = users.filter(({age}) => age > 30)
console.log(output); // [{firstName: 'Jack', lastName: 'Doe', age: 35}, {firstName: 'Jill', lastName: 'Doe', age: 40}, {firstName: 'Joe', lastName: 'Doe', age: 45}]
```

In the above code, `users` is an array of objects representing users. Each object has three properties: `firstName`, `lastName`, and `age`.

`filter` is called on the `users` array and it applies a callback function to each element in the array. 

The function takes a single argument, an object destructured to a single property `age`. This function checks if `age` is greater than 30. If it is, the function returns `true`, otherwise it returns `false`.

When `filter` is called on `users`, it applies this function to each element in the array, creating a new array with only the elements that returned `true`when passed to the function and returns the result. The original `users`array remains unchanged.

### **How to use `reduce()` in JavaScript**

The `reduce()` method is kind of overwhelming. If you have came across `reduce()` method before and haven't understood it at first, it's totally fine.

But don't worry – here, we will learn it through quite a few examples and I will try my best to make you understand this method.

Now, one doubt that might comes to your mind is why we use the `reduce()`method. As there are already lots of methods, how can we decide which one to use and when?

In the case of the `reduce()` method, you should is used it when you want to perform some operation on the elements of an array and return a single value as a result. The "single value" refers to the accumulated result of repeatedly applying a function to the elements of a sequence.

For example, you might use `reduce()` to sum up all the elements in an array, to find the maximum or minimum value, to merge multiple objects into a single object, or to group different elements in an array. 

Now let's understand all these with the help of examples.

**Example 1**: Using `reduce()` to sum up all the elements in an array:

```js
const numbers = [1, 2, 3, 4, 5];

const sum = numbers.reduce((total, currentValue) => {
    return total + currentValue;
}, 0)

console.log(sum); // 15
```

In this example, the `reduce()` method is called on the `numbers` array and is passed a callback function that takes two arguments: `total` and `currentValue`.

The `total` argument is the accumulation of the values that have been returned from the function so far, and the `currentValue` is the current element being processed in the array. 

The `reduce()` method also takes an initial value as the second argument, in this case `0`, which is used as the initial value of `total` for the first iteration.

In each iteration, the function adds the current value to the total and returns the new value of the total. 

The `reduce()` method then uses the returned value as the `total` for the next iteration, until it has processed all the elements in the array. 

Finally, it returns the final value of the total, which is the sum of all the elements in the array.

**Example 2**: Using `reduce()` to find the maximum value in an array:

```js
let numbers = [5, 20, 100, 60, 1];
const maxValue = numbers.reduce((max, curr) => {
    if(curr > max) max = curr;
    return max;
});
console.log(maxValue); // 100
```

In this example, again we have two arguments `max` and `curr` in the callback function. Notice we haven't passed the second parameter in the `reduce()`method this time. So, the default value will be the first element in the array.

The callback function first checks if the current element `curr` is greater than the current maximum value `max`. If it is, it updates the value of `max` to be the current element. If it is not, `max` is not updated. Finally, the function returns the value of `max`.

In this case, the `reduce()` method will start by setting `max` to 5 and `curr` to 20. It will then check if 20 is greater than 5, which it is, so it updates `max` to 20.

It will then set `curr` to 100 and check if 100 is greater than 20, which it is, so it updates `max` to 100.

It will continue this process until it has processed all the elements in the array. The final value of `max` will be the maximum value in the array, which is 100 in this case.

**Example 3**: Using `reduce()` to merge different objects in a single object:

```js
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const obj3 = { e: 5, f: 6 };

const mergedObj = [obj1, obj2, obj3].reduce((acc, curr) => {
    return { ...acc, ...curr };
}, {});

console.log(mergedObj); // { a: 1, b: 2, c: 3, d: 4, e: 5, f: 6 }
```

In this example, we have two arguments `acc` and `curr` in the callback function. The `acc` represents the current merged object that has been created so far, while the `curr` represents the current object being processed in the array.

The function uses the spread operator (`...`) to create a new object that combines the properties of the current merged object `acc` and the current object `curr`. It then returns this new object.

In this case, the `reduce()` method will start by setting `acc` to an empty object (which is the value passed as the second argument to `reduce()`). It will then set `curr` to `obj1`, and create a new object that combines the properties of the empty object and `obj1`. It will then set `curr` to `obj2` and create a new object that combines the properties of the previous merged object and `obj2`. It will continue this process until it has processed all the objects in the array.

The final value of `acc` will be the merged object, which will contain all the properties of the original objects.

**Example 4**: Using `reduce()` to group objects in an array. For example, grouping products in a shopping cart according to their brand name.

```js
const shoppingCart = [
    {name: 'Apple', price: 1.99, quantity: 3},
    {name: 'Apple', price: 1.99, quantity: 3},
    {name: 'Xiomi', price: 2.99, quantity: 2},
    {name: 'Samsung', price: 3.99, quantity: 1},
    {name: 'Tesla', price: 3.99, quantity: 1},
    {name: 'Tesla', price: 4.99, quantity: 4},
    {name: 'Nokia', price: 4.99, quantity: 4},
]

const products = shoppingCart.reduce((productGroup, product) => {
    const name = product.name;
    if(productGroup[name] == null) {
        productGroup[name] = [];
    }
    productGroup[name].push(product);
    return productGroup;
}, {});

console.log(products);
```

```json
// OUTPUT
{
  Apple: [
    { name: 'Apple', price: 1.99, quantity: 3 },
    { name: 'Apple', price: 1.99, quantity: 3 }
  ],
  Xiomi: [ { name: 'Xiomi', price: 2.99, quantity: 2 } ],
  Samsung: [ { name: 'Samsung', price: 3.99, quantity: 1 } ],
  Tesla: [
    { name: 'Tesla', price: 3.99, quantity: 1 },
    { name: 'Tesla', price: 4.99, quantity: 4 }
  ],
  Nokia: [ { name: 'Nokia', price: 4.99, quantity: 4 } ]
}
```

In this example, we have `shoppingCart` array representing different products and two arguments `productGroup` and `product` in the callback function. 

The `productGroup` argument represents the current group of products that have been found so far, while the `product` argument represents the current product being processed in the array.

The function first gets the name of the current product using `product.name`. It then checks if there is already a group for this product name in the `productGroup` object using the `if` statement. If there is not, it creates a new group by initializing the `productGroup[name]` property to an empty array.

Finally, the function pushes the current product into the group using the `push()` method, and returns the updated `productGroup` object.

After the `reduce()` method has processed all the elements in the `shoppingCart` array, the resulting `productGroup` object will contain keys for each product name, and values that are arrays of products with that name.

## Benefits of Higher Order Functions

Using higher order functions has some important benefits for web developers. 

First, higher order functions can help improve the legibility of your code by making it more concise and easy to understand. This can help speed up the development process and make it easier to debug code. 

Second, higher order functions can help organize your code into smaller chunks, making it easier to maintain and extend.
### References

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce

https://www.youtube.com/watch?v=7BeT6lsudL4&list=PL0Zuz27SZ-6Oi6xNtL_fwCrwpuqylMsgT&index=25



