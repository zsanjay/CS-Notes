
```js
// Recursion occurs when a function calls itself.

// Any iterator function (aka function with a loop)

//can be recursive instead.


// iterator function

const countToTen = (num = 1) => {

while(num <= 10) {

console.log(num);

num++;

}

}

  

//countToTen();

  

// recursive functions have 2 parts:

// 1. the recursive call to the function - expression or subproblem

// 2. at least one condition to exit - base case

  

const recurToTen = (num = 1) => {

  

if(num > 10) return;

console.log(num);

recurToTen(num + 1);

}

  

recurToTen();

  

// Reasons to use (not abuse) Recursion

// 1) Less Code

// 2) Elegant Code ( aka Pleasing to Look at)

// 3) Increased Readability

  

// Reasons to NOT use Recursion

// 1) Performance

// 2) Possibly more difficult to debug (aka follow the logic)

// 3) Is the Readability Improved?

  

// The Fibonacci Sequence

// 0, 1, 1, 2, 3, 5, 8, 13, 21, etc.

  

// Without Recursion

const fibonacci = (num , array = [0 , 1]) => {

while(num > 2) {

const [nextToLast, last] = array.slice(-2);

array.push(nextToLast + last);

num -= 1;

}

return array;

}

  

//console.log(fibonacci(12));

  

// With Recursion

const recursiveFibonacci = (num , array = [0 , 1]) => {

if(num <= 2) return array;

const [nextToLast, last] = array.slice(-2);

return recursiveFibonacci(num - 1, [...array , nextToLast + last]);

}

  

console.log(recursiveFibonacci(12));

  

// What number is in the nth position of the Fibonacci Sequence?

  

// Without Recursion

const fibonacciPos = (pos) => {

if(pos <= 1) return pos;

const seq = [0, 1];

for(let i = 2; i <= pos; i++) {

const [nextToLast , last] = seq.slice(-2);

seq.push(nextToLast + last);

}

return seq[pos];

}

  

console.log(fibonacciPos(8));

  

// With Recursion

const recurFibonacciPos = (pos , dp) => {

if (pos < 2) return pos;

if(dp[pos]) {

return dp[pos];

}

let a = recurFibonacciPos(pos - 1 , dp);

let b = recurFibonacciPos(pos - 2, dp);

dp[pos] = a + b;

return a + b;

}

  

console.log(recurFibonacciPos(8 , []));

  

// One line code

const fibPos = pos => pos < 2 ? pos : fibPos(pos - 1) + fibPos(pos - 2);

console.log(fibPos(8));

  

// 2) A Parser:

// a company directory

// a file directory

// the DOM - a web crawler

// An XML or JSON data export

  

// Export from your streaming service like Spotify, YT Music, etc.

  

const artistsByGenre = {

jazz: ["Miles Davis", "John Coltrane"],

rock: {

classic: ["Bob Seger", "The Eagles"],

hair: ["Def Leppard", "Whitesnake", "Poison"],

alt: {

classic: ["Pearl Jam", "The Killers"],

current: ["Joywave", "Sir Sly"]

}

},

unclassified: {

new: ["Caamp", "Neil Young"],

classic: ["Seal", "Morcheeba", "Chris Stapleton"]

}

}

  

const getArtistsNames = (artistsByGenre , artists = []) => {

Object.keys(artistsByGenre).forEach(key => {

if(Array.isArray(artistsByGenre[key])) {

artistsByGenre[key].forEach(artist => {

artists.push(artist);

});

return artists;

}

getArtistsNames(artistsByGenre[key], artists);

});

return artists;

}

console.log(getArtistsNames(artistsByGenre));
```


