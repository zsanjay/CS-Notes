
1. Arithmetic Operators

An expression in JavaScript is something that produces a value.An expression goes on the right side of our variable assignment.

In JavaScript, we have different arithmetic operators.

% is the modulo operator and it returns the remainder of division.

```js
- , + , * , / , %

console.log(2 + 2); // 4

console.log(4 - 2); // 2

console.log(2 * 2); // 4

console.log(9 / 3); // 3

console.log(10 / 3); // 3.3333333333333335

console.log(10 % 2); // the remainder of 0

console.log(3 ** 2); // 3^2 = 9 (raised to the power of)
```

We also have shorthand (syntactic sugar) to increment or decrement a value.

```js
let a = 10;
a = a + 1;
a += 1;
a -= 1;
```

This will increment the value of a, and then return the previous value.
```js
a++; 
a--;
```

This will increment the value of a, and then return the updated value.
```js
++a;
--a;
```

2. Assignment Operator

The assignment operator is a single equal sign, =, which doesn't check for equality.

```js
let programmingLanguage = 'JavaScript';
```

3. Comparison Operator

Comparison operators includes :

'>', greater than
'>=', greater than or equal to
<, less than
<=, less than or equal to

// The result of a comparison operator is a boolean value, true or false.

```js
let num1 = 14;
let num2 = 10;

const isNum1Greater = num1 > num2;
console.log('isNum1Greater', isNum1Greater); // true

const isNum1GreaterThanOrEqualTo = num1 >= num2;
console.log('isNum1GreaterThanOrEqualTo', isNum1GreaterThanOrEqualTo); // true

const isNum1LessThan = num1 < num2;
console.log('isNum1LessThan', isNum1LessThan); // false

const isNum1LessThanOrEqualTo = num1 <= num2;
console.log('isNum1LessThanOrEqualTo', isNum1LessThanOrEqualTo); // false
```

4. Equality Operator

For checking for equality, we can do so through...
1. Loose equality, which uses ==
2. Strict equality, which uses ===

Loose equality evaluates this comparison to be truthy.
Loose equality doesn't check if the data types are the same.
JavaScript will perform type coercion with loose equality, converting the data types to be the same before comparing.

```js
let a = 2;
let b = '2';

console.log(a == b); // true
```

Strict equality returns true if the data types are the same and the the values are the same.

Stick to strict equality and use === for equality comparisons.

```js
console.log(a === b); // false

console.log(true == '1'); // true
```

5. Ternary Operator

In JavaScript, we have the ternary operator.
It is a conditional operator that allows us to write cleaner code.
This can be used when you need to perform a comparison and store values.

```js
// Note: in this case you could just do 
// const canDrive = age >= 16; 
// but this is just to demonstrate the syntax of the ternary operator.

let age = 16;
const canDrive = age >= 16 ? true : false;
console.log('canDrive', canDrive);

let points = 110;
const customerType = points > 100 ? 'gold' : 'silver';
console.log('customerType', customerType);

```

6. Logical Operator

We use logical operators to make decisions based on multiple conditions.

There are 4 logical operators which include:
1. || (or operator)
2. && (and operator)
3. !  (not operator)
4. ?? (null coalescing)

These can be applied to values of any type, not just boolean values. Expressions are evaluated from left to right.

Consider the four possible logical combinations that can be made when working with two operands. So as long as one is true, then it will return true.

```js
console.log(true || true); // true
console.log(true || false); // true
console.log(false || true); // true
console.log(false || false); // false
```

An example of the ||, or operator. 

```js
let hasReservation = true;
let acceptingWalkIns = false;
const hasAccessToTable = hasReservation || acceptingWalkIns;

console.log('hasAccessToTable', hasAccessToTable); // true
```

&&, the AND operator

This returns true if all the operands being evaluated are truthy.
Consider the four possible logical combinations that can be made when working with two operands.

```js
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false
console.log(false && false); // false

let age = 16;
let hasCar = true;
const canDrive = age >= 16 && hasCar;

console.log('canDrive', canDrive); // true

// Another example
let a = true, b = true, c = true, d = true;

console.log(a && b || c && d); // true

// Same as...
console.log((a && b) || (c && d)); // true
```

!, the NOT operator.

This will return the inverse of the operand.

```js
console.log(!true); // false

// Another example
let isClosedOnSunday = true;

const isRestaurantOpen = !isClosedOnSunday;

console.log(isRestaurantOpen); // false
```

??, nullish coalescing operator.

So if a value is null or undefined, then we can supply a default value.

```js
let doesValueExist = null;
const result = doesValueExist ?? false;

// So the ?? operator is syntactic sugar for...
const resultOfExpression = (a !== null && a !== undefined) ? a : false;
```

Logical operators with non booleans:

Expressions are evaluated from left to right. When using logical operators with non-boolean values, rather than returning the value of true or false, it will return the value of the operand.

 || (or operator)

```js
console.log(false || 'Steven'); // returns 'Steven'
```

So since 'Steven' is evaluated to truthy, it will be the value returned the OR operator returns the first truthy value.

##### The falsy values in JavaScript are:

	undefined, null, 0, false, '', NaN

anything else that doesn't fall in this category is considered truthy.

```js
console.log(false || 1 || 2); // returns 1
```


The JavaScript OR operator, ||, performs short-circuit evaluation. Meaning it stops the expression once it can evaluate to 'truthy' or 'falsy'.

Another example:

```js
let usersChosenColor = 'blue';
let defaultColor = 'green';

const currentWebsiteColor = usersChosenColor || defaultColor;
console.log(currentWebsiteColor); // blue
```

&& (AND operator)

The **logical AND (`&&`)** (logical conjunction) operator for a set of boolean operands will be `true` if and only if all the operands are `true`. Otherwise it will be `false`.

More generally, the operator returns the value of the first [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) operand encountered when evaluating from left to right, or the value of the last operand if they are all [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy).

```js
const a = 3;
const b = -2;

console.log(a > 0 && b > 0);
// Expected output: false
```

Examples of expressions that can be converted to false are:

- `false`;
- `null`;
- `NaN`;
- `0`;
- empty string (`""` or `''` or ` `` `);
- `undefined`.

The AND operator preserves non-Boolean values and returns them as they are:

```js
result = "" && "foo"; // result is assigned "" (empty string)
result = 2 && 0; // result is assigned 0
result = "foo" && 4; // result is assigned 4
```

More Examples

```js
function A() {
  console.log("called A");
  return false;
}
function B() {
  console.log("called B");
  return true;
}

console.log(A() && B());
// Logs "called A" to the console due to the call for function A,
// && evaluates to false (function A returns false), then false is logged to the console;
// the AND operator short-circuits here and ignores function B
```

```js
a1 = true && true; // t && t returns true
a2 = true && false; // t && f returns false
a3 = false && true; // f && t returns false
a4 = false && 3 === 4; // f && f returns false
a5 = "Cat" && "Dog"; // t && t returns "Dog"
a6 = false && "Cat"; // f && t returns false
a7 = "Cat" && false; // t && f returns false
a8 = "" && false; // f && f returns ""
a9 = false && ""; // f && f returns false
```

#### Operator Precedence:

Follow the PEMDAS - Parentheses, Exponent, Multiplication, Divide, Add and Subtract.

```js
let n = 5 + 5 * 4;
console.log(n); // 25
```

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_precedence

