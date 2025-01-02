
Share properties among many objects of the same type.

It we want to share properties among many objects of the same type, we can use the Prototype pattern.

![[20241231172428.png]]
#### Implementation

Say we wanted to create many dogs with a createDog factory function.

```js
const createDog = (name, age) => ({
  name,
  age,
  bark() {
    console.log(`${name} is barking!`);
  },
  wagTail() {
    console.log(`${name} is wagging their tail!`);
  },
});

const dog1 = createDog("Max", 4);
const dog2 = createDog("Sam", 2);
const dog3 = createDog("Joy", 6);
const dog4 = createDog("Spot", 8);
```

This way, we can easily create many dog objects with the same properties.

![[20241231172831.png]]
However, we're unnecessarily adding a new bark and wagTail methods to each dog object. Under the hood, we're creating two new functions for each dog object, which uses memory.

We can use the Prototype Pattern to share these methods among many dog objects.

```js
class Dog {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  bark() {
    console.log(`${this.name} is barking!`);
  }
  wagTail() {
    console.log(`${this.name} is wagging their tail!`);
  }
}

const dog1 = new Dog("Max", 4);
const dog2 = new Dog("Sam", 2);
const dog3 = new Dog("Joy", 6);
const dog4 = new Dog("Spot", 8);

console.log({ dog1, dog2, dog3, dog4 });
/*
{
  dog1: Dog { name: 'Max', age: 4 },
  dog2: Dog { name: 'Sam', age: 2 },
  dog3: Dog { name: 'Joy', age: 6 },
  dog4: Dog { name: 'Spot', age: 8 }
}
*/
  
dog1.bark(); // Max is barking!
```

![[20241231173226.png]]
ES6 classes allow us to easily share properties among many instances, bark and wagTail in this case.

### Tradeoffs

#### Pros

**Memory efficient**: The prototype chain allows us to access properties that aren't directly defined on the object itself, we can avoid duplication of methods and properties, thus reducing the amount of memory used.

#### Cons

**Readability**: When a class has been extended many times, it can be difficult to know where certain properties come from.

For example, if we have a `BorderCollie` class that extends all the way to the `Animal` class, it can be difficult to trace back where certain properties came from.

![[20241231173742.png]]
#### Exercise

#### Challenge

Refactor this factory function to a class.

```js
const createUser = ({ firstName, lastName, email }) => ({
	firstName,
	lastName,
	fullName: `${firstName} ${lastName}`,
	email,
	checkLastOnline: () => {
		console.log(`${this.fullName} was last online at ${Date.now()}`);
	},
	
	sendEmail: () => {
		console.log(`Email sent to ${email}`);
	},
	
	delete: () => {
		console.log('User removed');
	},
});

const user = createUser({
	firstName: 'John',
	lastName: 'Doe',
	email: 'john@doe.com',
});

const user2 = createUser({
	firstName: 'Jane',
	lastName: 'Doe',
	email: 'jane@doe.com',
});

console.log(user.delete === user2.delete); // false
```


#### Solution

```js
class User {
	constructor({firstName, lastName, email}) {
		this.firstName = firstName;
		this.lastName = lastName;
		this.fullName = `${this.firstName} ${this.lastName}`;
		this.email = email;
	}
	
	checkLastOnline() {
		console.log(`${this.fullName} was last online at ${Date.now()}`);
	}
	
	sendEmail() {
		console.log(`Email sent to ${email}`);
	}
	
	delete() {
		console.log('User removed');
	}
}

const user = new User({
	firstName: 'John',
	lastName: 'Doe',
	email: 'john@doe.com'
});

const user2 = new User({
	firstName: 'Jane',
	lastName: 'Doe',
	email: 'jane@doe.com'
});

console.log(user.delete === user2.delete); // true
```
