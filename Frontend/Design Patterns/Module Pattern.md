Split up your code into smaller, reusable pieces.

ES2015 introduced built-in JavaScript modules. A module is a file containing JavaScript code and makes it easy to expose and hide certain values.

The module pattern is a great way to split a larger file into **multiple smaller**, **reusable pieces**. It also promotes **code encapsulation**, since the values within modules are kept private inside the module by default, and cannot be modified. Only the values that are explicitly exported with the `export` keyword are accessible to other files.

![[20241231104131.png]]

![[20241231104040.png]]\

The module pattern provides a way to have both public and private pieces with the `export` keyword. This protects values from leaking into the global scope or ending up in a naming collision.

![[20241231104401.png]]
In the above example, the `secret` variable is accessible within the module, but not outside of it. Other modules cannot import this value, as it hasn't been `export`ed.

Without modules, we can access properties defined in scripts that are loaded prior to the currently running script.

Math.js
```js
function sum(x, y) {
	return x + y;
}

function multiply(x, y) {
	return x * y;
}

function subtract(x, y) {
	return x - y;
}

function divide(x, y) {
	return x / y;
}
```

index.js
```js
console.log('Sum', sum(1, 2)); // 3
console.log('Subtract', subtract(1, 2)); // -1
console.log('Divide', divide(1, 2)); // 0.5
console.log('Multiply', multiply(1, 2)); // 2
```

index.html
```js
<html>

<head>
<meta charset="UTF-8" />
<script src="math.js"></script>
<script src="index.js"></script>
<link href="./styles.css" rel="stylesheet" />
</head>

<body>
<div class="card">↓ Open the console ↓</div>
</body>

</html>
```

With modules however, we can only use the values that have been exported.

#### Implementation

There are a few ways we can use modules.
#### HTML tag

When adding JavaScript to HTML files directly, you can use modules by adding the `type="module"` attribute to the `script` tags.

![[20241231115715.png]]

```js
import { sum, subtract, divide, multiply } from './math.js';

console.log('Sum', sum(1, 2)); // 3
console.log('Subtract', subtract(1, 2)); // -1
console.log('Divide', divide(1, 2)); // 0.5
console.log('Multiply', multiply(1, 2)); // 2
```

#### Node

In Node, you can use ES2015 modules either by:

- Using the `.mjs` extension
- Adding `"type": "module"` to your `package.json`

package.json
```json
{
"name": "node-starter",
"version": "0.0.0",
"type": "module",
"scripts": {
	"test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

If you don't add "type": "module", you will get an error saying:
```
(node:10) Warning: To load an ES module, set "type": "module" in the package.json or use the .mjs extension.
```

#### Tradeoffs

**Encapsulation**: The values within a module are scoped to that specific module. Values that aren't explicitly exported are not available outside of the module.

**Reusability**: We can reuse modules throughout our application.