	 Lexical Scope defines how variable names are resolved in nested functions.
	
	 Nested (child) functions have access to the scope of their parent functions.
	
	This is often confused with closure, but lexical scope is only an important       part of closure.

#### A Closure is a function having access to the parent scope, even after the parent function has closed.

#### A closure is created when we define a function, not when a function is executed.

```js
// Global Scope
let x = 1;

const parentFn = () => {

// Local Scope
let myValue = 2;

console.log(x);
console.log(myValue);

const childFn = () => {
	console.log(x += 5);
	console.log(myValue += 1);
}
return childFn;
}

const result = parentFn();
console.log(result);

result(); // x = 6, myValue = 3
result(); // x = 11, myValue = 4
result(); // x = 16, myValue = 5
console.log(x); // x = 16
```

### References

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#








