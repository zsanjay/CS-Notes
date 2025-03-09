ES6 (ECMAScript 2015) introduced several powerful features to JavaScript that make development more efficient and readable. Here are some key ES6 features you should know:

### 1. **let and const (Block-scoped variables)**

- `let`: Mutable variable with block scope.
- `const`: Immutable variable (cannot be reassigned).

```js
let x = 10; x = 20; // Allowed 
const y = 30; y = 40; // Error: Assignment to constant variable.
```

### 2. **Arrow Functions**

- A shorter syntax for functions.
- Does not bind its own `this`.

```js
const add = (a, b) => a + b;
console.log(add(5, 3)); // 8
```

### 3. **Template Literals**

- Multi-line strings and string interpolation.

```js
const name = "John";
console.log(`Hello, ${name}!`); // Hello, John!
```

### 4. **Destructuring Assignment**

- Extract values from arrays or objects.

```js
const person = { name: "Alice", age: 25 };
const { name, age } = person;
console.log(name, age); // Alice 25

const numbers = [1, 2, 3];
const [first, second] = numbers;
console.log(first, second); // 1 2
```

### 5. **Default Parameters**

- Set default values for function parameters.

```js
function greet(name = "Guest") {
  console.log(`Hello, ${name}!`);
}
greet(); // Hello, Guest!
```

### 6. **Spread and Rest Operator (`...`)**

- **Spread (`...`)**: Expands an array or object.
- **Rest (`...`)**: Gathers remaining arguments.

```js
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];
console.log(arr2); // [1, 2, 3, 4, 5]

function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}
console.log(sum(1, 2, 3)); // 6
```

### 7. **Enhanced Object Literals**

- Shorthand for defining objects.

```js
const age = 30;
const user = { name: "John", age };
console.log(user); // { name: "John", age: 30 }
```

### 8. **Promises**

- Handles asynchronous operations.

```js
const fetchData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("Data fetched"), 1000);
  });
};

fetchData().then(data => console.log(data)); // Data fetched
```

### 9. **Classes**

- Introduces OOP-like class syntax.

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
}

const john = new Person("John");
john.greet(); // Hello, my name is John
```

### 10. **Modules (`import/export`)**

- Enables modular JavaScript.

```js
// utils.js
export const sum = (a, b) => a + b;

// main.js
import { sum } from "./utils.js";
console.log(sum(3, 4)); // 7
```

These ES6 features greatly improve code readability, maintainability, and performance. 🚀 Let me know if you need examples or explanations on any of these!