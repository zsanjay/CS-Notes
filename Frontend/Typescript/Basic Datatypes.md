
TypeScript stands in an unusual relationship to JavaScript. TypeScript offers all of JavaScript’s features, and an additional layer on top of these: TypeScript’s type system.

For example, JavaScript provides language primitives like `string` and `number`, but it doesn’t check that you’ve consistently assigned these. TypeScript does.

This means that your existing working JavaScript code is also TypeScript code. The main benefit of TypeScript is that it can highlight unexpected behavior in your code, lowering the chance of bugs.

```tsx
let myName : string = 'Dave';
let meaningOfLife: number;
let isLoading : boolean;

//let album : any;
let album : string | number;

myName = 'John';
meaningOfLife = 42;
isLoading = true;
album = 45;


const sum = (a : number, b : number) => a + b;


let postId : string | number
let isActive : number | boolean | string

let re = /\w+/g
```
## Types by Inference

TypeScript knows the JavaScript language and will generate types for you in many cases. For example in creating a variable and assigning it to a particular value, TypeScript will use the value as its type.

```tsx
let helloWorld = "Hello World"; // let helloWorld: string
```

## Defining Types

You can use a wide variety of design patterns in JavaScript. However, some design patterns make it difficult for types to be inferred automatically (for example, patterns that use dynamic programming). To cover these cases, TypeScript supports an extension of the JavaScript language, which offers places for you to tell TypeScript what the types should be.

For example, to create an object with an inferred type which includes `name: string` and `id: number`, you can write:

```jsx
const user = {
	name: "Hayes",
	id: 0,
};
```

You can explicitly describe this object’s shape using an `interface` declaration:

```tsx
interface User {
	name: string;
	id: number;
}
```

You can then declare that a JavaScript object conforms to the shape of your new `interface` by using syntax like `: TypeName`after a variable declaration:

```tsx
const user: User = {
	name: "Hayes",
	id: 0,
};
```

If you provide an object that doesn’t match the interface you have provided, TypeScript will warn you:

```tsx
interface User {
	name: string;
	id: number;
}

const user: User = {
	username: "Hayes",
	Object literal may only specify known properties, and 'username' does not exist in type 'User'.
	
	id: 0,
};
```

```tsx
interface User {
	name: string;
	id: number;
}

class UserAccount {
	name: string;
	id: number;
	constructor(name: string, id: number) {
		this.name = name;
		this.id = id;
	}
}

const user: User = new UserAccount("Murphy", 1);
```

You can use interfaces to annotate parameters and return values to functions:

```tsx
function deleteUser(user: User) {
	// ...
}

function getAdminUser(): User {
	//...
}
```

There is already a small set of primitive types available in JavaScript: `boolean`, `bigint`, `null`, `number`, `string`, `symbol`, and `undefined`, which you can use in an interface. TypeScript extends this list with a few more, such as `any` (allow anything), [`unknown`](https://www.typescriptlang.org/play#example/unknown-and-never) (ensure someone using this type declares what the type is), [`never`](https://www.typescriptlang.org/play#example/unknown-and-never) (it’s not possible that this type could happen), and `void` (a function which returns `undefined` or has no return value).

You’ll see that there are two syntaxes for building types: [Interfaces and Types](https://www.typescriptlang.org/play/?e=83#example/types-vs-interfaces). You should prefer `interface`. Use `type` when you need specific features.

## Composing Types

With TypeScript, you can create complex types by combining simple ones. There are two popular ways to do so: unions and generics.

### [Unions](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#unions)

With a union, you can declare that a type could be one of many types. For example, you can describe a `boolean` type as being either `true` or `false`:

```tsx
type MyBool = true | false;   
```

A popular use-case for union types is to describe the set of `string` or `number` [literals](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) that a value is allowed to be:

```tsx
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

Unions provide a way to handle different types too. For example, you may have a function that takes an `array` or a `string`:

```tsx
function getLength(obj: string | string[]) {
	return obj.length;
}
```
### Generics

Generics provide variables to types. A common example is an array. An array without generics could contain anything. An array with generics can describe the values that the array contains.

```tsx
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```


