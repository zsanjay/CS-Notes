
Intercept and control interactions to target objects.

The Proxy Pattern uses a Proxy intercept and control interactions to target objects.

Let's say that we have a person object. We can access properties with either dot or bracket notation,

![[20241231125936.png]]

 and modify property values in a similar fashion.

![[20241231130623.png]]

With the Proxy pattern, we don't want to interact with this object directly. Instead, a Proxy object intercepts the request, and (optionally) forwards this to the target object - the person object in this case.

![[20241231130942.png]]

![[20241231131014.png]]

#### Implementation

In JavaScript, we can easily create a new proxy by using the built-in Proxy object.

![[20241231131448.png]]
The Proxy object receives two arguments:

1. The target object
2. A handler object, which we can use to add functionality to the proxy. This object comes with some built-in functions that we can use, such as get and set.

The get method on the handler object gets invoked when we want to access a property, and the set method gets invoked when we want to modify a property.

```js
const person = {
  name: "John Doe",
  age: 42,
  email: "john@doe.com",
  country: "Canada",
};

const personProxy = new Proxy(person, {
  get: (target, prop) => {
    console.log(`The value of ${prop} is ${target[prop]}`);
    return target[prop];
  },
  set: (target, prop, value) => {
    console.log(`Changed ${prop} from ${target[prop]} to ${value}`);
    target[prop] = value;
    return true;
  },
});
```

#### Reflect 

The built-in Reflect object makes it easier to manipulate the target object.

Instead of accessing properties through obj[prop] or setting properties through obj[prop] = value, we can access or modify properties on the target object through Reflect.get() and Reflect.set(). The methods receive the same arguments as the methods on the handler object.

![[20241231134529.png]]

#### Tradeoffs
\
#### Pros

**Control**: Proxies make it easy to add functionality when interacting with a certain object, such as validation, logging, formatting, notifications, debugging.

#### Cons

**Long handler execution**: Executing handlers on every object interaction could lead to performance issues.

#### Exercise

#### [Challenge](https://javascriptpatterns.vercel.app/patterns/design-patterns/proxy-pattern#challenge)

Add the following validation to the `user` object:

- The `username` property has to be a `string` that only contains of letters, and is at least 3 characters long
- The `email` property has to be a valid email address.
- The `age` property has to be a number, and has to be at least `18`
- When a property is retrieved, change the output to `${new Date()} | The value of ${property}} is ${target[property]}`. For example if we get `user.name`, it needs to log `2022-05-31T15:29:15.303Z | The value of name is John`.

validator.js

```js
export function isValidEmail(email) {
	const tester =
/^[-!#$%&'*+\/0-9=?A-Z^_a-z`{|}~](\.?[-!#$%&'*+\/0-9=?A-Z^_a-z`{|}~])*@[a-zA-Z0-9](-*\.?[a-zA-Z0-9])*\.[a-zA-Z](-?[a-zA-Z0-9])+$/;

	return tester.test(email);
}

export function isAllLetters(char) {
	if (typeof char !== 'string') {
		return false;
	}
	return /^[a-zA-Z]+$/.test(char);
}
```

index.js

```js
import { isValidEmail, isAllLetters } from './validator.js';

const user = {
	firstName: 'John',
	lastName: 'Doe',
	username: 'johndoe',
	age: 42,
	email: 'john@doe.com'
};

const userProxy = new Proxy(user, {
	get: (target, prop) => {
		return `${new Date()} | The value of ${prop}} is ${Reflect.get(
		target,
		prop
	)}`;
},

set: (target, prop, value) => {
	if (prop === 'email') {
		if (!isValidEmail(email)) {
			console.log('Please provide a valid email.');
			return false;
		}
	}

	if (prop === 'age') {
		if (typeof value !== 'number') {
		throw new Error('Please provide a valid age.');
	}
	
	if (value < 18) {
		throw 'User cannot be younger than 18.';
	}
}

if (prop === 'username') {
	if (value.length < 3) {
		throw new Error('Please use a longer username.');
	} else if (!isAllLetters(value)) {
		throw new Error('You can only use letters');
	}
}

return Reflect.set(target, prop, value);

},
});
```





