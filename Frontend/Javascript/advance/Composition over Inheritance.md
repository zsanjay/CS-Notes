
**Inheritance** is one of the core concepts of object-oriented programming, that helps us, developers to avoid code duplication. The main idea is that we create a base class, which contains logic, that will be reused by our subclasses. Let’s see an example:

![]()![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-x393mh9.png?w=800&q=75&auto=format)

From the example, we can see that we have a base class **“_Element_”** and all our future elements will extend common logic from the Element class.

Also, maybe you heard **is-a** relationship. What does it mean? For example, “_**Form**_” is an “_**Element**_” and “_**Button**_” is an “_**Element**_”.

Another example, we can create a base class “_**Animal**_” and our subclasses like “_**Bird**_”, “_**Dog**_”, “_**Cat**_” also has **is-a** relationship. Why? Because “_**Bird**_” is an “_**Animal”**_, “_**Dog**_” is an “_**Animal**_” and “_**Cat**_” is also an “_**Animal**_”.

And now, let’s speak a little bit about _**composition**_. Unlike inheritance, the composition uses a **has-a** relationship. We collect different pieces of functionality together. Let’s see an example:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-tpa3ml0.png?w=800&q=75&auto=format)

In this example, we can see that we have main class “_**Car**_” that uses “_**Engine**_” and  “_**Transmission**_”.
  ![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-whb3mty.png?w=800&q=75&auto=format)

In this example, we can’t say that “_**Engine**_“ is a “_**Car**_” or “_**Transmission**_” is a “_**Car**_”. But, we can say that “_**Car**_” has “_**Transmission**_” or “_**Car**_” has “_**Engine**_”.

I hope that you understand what is _**Inheritance**_ and what is _**Composition**_ if you forget.

So, now, let’s look at different examples. First, we look at an example using the class way approach, and then, we will take a look at the function way approach.

Imagine, we are working with the file system and we want to create read, write and remove functionality. What can we do? Just, for example, we can create a class, that will do this, and it’s ok:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-8i93gqr.png?w=800&q=75&auto=format)

I think for this moment everything is ok with this approach.

But, a little bit later, we understand, that we want to have permissions, and some of our users will have just read permission, another one will have write permission. What can we do? One solution is can divide our methods, into different classes, like:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-5593hqp.png?w=800&q=75&auto=format)

And now, we can use the needed permission for each client. But, now, we have a problem. What, if we need to give someone permission for the reading and for the writing? Or for the reading and for the removing? With the current implementation, we can’t do it. How we can solve it?

The first solution could be: to create a class for reading and writing, or for reading and for removing:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-oza3hs7.png?w=800&q=75&auto=format)

If we do this, we will have next classes: FileReader, FileWriter, FileRemove, FileReaderAndWriter, FileReaderAndRemover.

And it’s bad. Why? The first reason is if we have not just 3 methods, but 10, 20. We can have a really big amount of combinations between them. The second reason is that we duplicate logic in our classes. Just, for example, our FileReader class contains read methods, and FileReaderAndWriter also contains duplicate code of the same method.

So, as you can see, it’s a not good solution.

Do we have another approach? How about multiple inheritances? First of all, we don’t have this feature in Javascript, but, if we do, it’s also not good, because we will have really confusing design when 1 class inherits from another, and those other, inherits from many other. It will be really awful code architecture.

So, what is the solution? One of the possible solutions here is to use composition. How it will look like?

First of all, let’s move our methods to a separate function factory. Why function? Because maybe we want to pass some parameters later. In our example, we don’t need it.

  
![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-tpb3hcz.png?w=800&q=75&auto=format)

In the above example, we have 2 functions that create objects with the reading or with the writing method.

What does it give us? Now, we can use them in any place where we want and combine them:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-mgc3hw2.png?w=800&q=75&auto=format)

In the example above, we created a read and write service. This is almost what we have at the start of our FileReader class (just without the “remove” method).

And now, if we want to combine different permission: for reading and writing, or writing and deleting, we can do it very easily:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-1xd3hhd.png?w=800&q=75&auto=format)

If we have 5, 10, 20 methods, we can combine them how we want. We don’t have a problem with duplication code, we don’t have confusing architecture.

Now, let’s see another example, without classes. Imagine, that we have employees. For example: taxi driver, sport coach, and manager:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-18e3h8r.png?w=800&q=75&auto=format)

And everything is ok now. But, imagine a situation, where some employee works as a sports coach, but sometimes or at the night works also as a taxi driver. What can we do?

Create a function like:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-spf3hm4.png?w=800&q=75&auto=format)

Yes, it will work, but the problems are the same as in the first example with classes. We could have really many different combinations with code duplication. So, knowing now, how composition works, we can refactor our code like here:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-ith3hpx.png?w=800&q=75&auto=format)

And now, we can combine all our work types as we want, without duplications and with an understandable approach:

![image](https://hackernoon.imgix.net/images/S0zuR5PfGFN2e1h8xuRZHD1SPBK2-2bi3htg.png?w=800&q=75&auto=format)

I really hope that now, you can see the difference between inheritance and composition.

In general, inheritance could answer the **“is-a”** relationship, when composition **“has-a”.**

But in practice, as we can see _**inheritance**_, is sometimes not a good approach. Just in our example, the driver is an employee and the manager also is an employee. But when our task is to combine different pieces into a single item, like here, _**composition**_ really could be much better than _**inheritance**_.

At the end of this article, I want to focus on that both _**inheritance**_ and **composition** are good approaches when you are using them in the right place when they should be. In one situation composition is better than inheritance and in another situation vice versa.

And of course, we can combine _**inheritance**_ and _**composition**_ together, if we have **“is-a”** relationship, but we want to add different values or methods, we can have some base class, that gives all common functionality for our instances, and we can also use composition to add other, specific functionality.

```js
// Why to choose composition over inheritance.

class Pizza {

constructor(size, crust, sauce) {
	this.size = size;
	this.crust = crust;
	this.sauce = sauce;
	this.toppings = [];
}

prepare() { console.log('Preparaing...') }

bake() { console.log('Baking...') }

ready() { console.log('Ready!') }

}

// Problem: Repeating methods - Not D.R.Y.
class Salad {

constructor(size, dressing) {
	this.size = size;
	this.dressing = dressing;
}

prepare() { console.log('Preparaing...') }

toss() { console.log('Tossing...') }

ready() { console.log('Ready!') }

}


class StuffedCrustPizza extends Pizza {
	stuff() { console.log('Stuffing the crust...') }
}

  
class ButterCrustPizza extends Pizza {
	butter() { console.log('Buttering the crust...') }
}

  
// Problem: Repeating methods - Not D.R.Y.
class StuffedButteredCrustPizza extends Pizza {
	stuff() { console.log('Stuffing the crust...') }
	butter() { console.log('Buttering the crust...') }
}

const myPizza = new StuffedButteredCrustPizza();

myPizza.stuff();
myPizza.butter();


// Instead, use composition for methods
const prepare = () => {
	return {
		prepare : () => console.log('Preparaing...');
	}
}

const bake = () => {
	return {
		bake: () => console.log('Baking...');
	}
}

const toss = () => {
	return {
		toss: () => console.log('Tossing...');
	}
}

const ready = () => {
	return {
		ready: () => console.log('Ready!');
	}
}

const stuff = () => {
	return {
		stuff() { console.log('Stuffing the crust...') }
	}
}

const butter = () => {
	return {
		butter() { console.log('Buttering the crust...') }
	}
}


const createPizza = (size, crust, sauce) => {
	const pizza = {
		size: size,
		crust: crust,
		sauce: sauce,
		toppings: []
	}
	  
	return {
		...pizza,
		...prepare(),
		...bake(),
		...ready()
	}
}


const createSalad = (size, dressing) => {
	return {
		size: size,
		dressing: dressing,
		...prepare(),
		...toss(),
		...ready()
	}
}


// Compare to ES6 Class syntax with extends and super()
const createStuffedButteredCrustPizza = (pizza) => {
	return {
		...pizza,
		...stuff(),
		...butter()
	}
}

  

const anotherPizza = createPizza("medium" , "thin", "original");

const somebodysPizza = createStuffedButteredCrustPizza(anotherPizza);
// OR
const davesPizza = createStuffedButteredCrustPizza(createPizza("medium" , "thin", "original"));

const sanjaysSalad = createSalad("side", "ranch");

davesPizza.bake();
davesPizza.stuff();
sanjaysSalad.prepare();

console.log(davesPizza);
console.log(sanjaysSalad);


// This is the impure function
const addTopping = (pizza, topping) => {
	pizza.toppings.push(topping);
	return pizza;
}


const jimsPizza = createPizza("medium" , "thin", "original");
console.log(jimsPizza);
console.log(addTopping(jimsPizza, "pepperoni")); // mutation!

// We need to clone the pizza object to avoid mutation
// Function composition:

/* const shallowPizzaClone = (fn) => {
	return (obj, array) => {
		const newObj = { ...obj };
		return fn(newObj, array);
	}
} */

const shallowPizzaClone = (fn) => (obj, array) => fn({ ...obj }, array);

// Pure Function
let addToppings = (pizza, toppings) => {
	pizza.toppings = [...pizza.toppings, ...toppings];
	return pizza;
}

// Decorate the addToppings function with shallowPizzaClone
addToppings = shallowPizzaClone(addToppings);

const timsPizza = createPizza("medium", "thin", "original");
const timsPizzaWithToppings = addToppings(timsPizza, ["olives", "cheese", "pepperoni"]);

console.log(timsPizzaWithToppings);
console.log(timsPizza);
console.log(timsPizzaWithToppings === timsPizza);
```

### References

https://hackernoon.com/inheritance-vs-composition-in-javascript

https://www.youtube.com/watch?v=8nckJU3dalc&list=PL0Zuz27SZ-6N3bG4YZhkrCL3ZmDcLTuGd&index=8