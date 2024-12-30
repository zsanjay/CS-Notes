
#### Falsy Values

1. undefined
2. null
3. 0
4. ''
5. NaN

```js

var user = null;

if(user) {
	console.log("Condition is true");
}

// Nothing will print

user = "null";
if(user) {
	console.log("Condition is true");
}

// Condition is true
```

Everything other than these values considered as a truthy values.

### Coercion

 ===  - strict check is called coercion.

```js
var user = "2";

if(2 === user) { // False
	console.log("Condition is true");
}

```







