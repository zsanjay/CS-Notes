Refer to MDN docs 
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes

Classes are a template for creating objects. They encapsulate data with code to work on that data. Classes in JS are built on [prototypes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) but also have some syntax and semantics that are unique to classes.
#### Class

```js
class Pizza {

	constructor(pizzaType , pizzaSize) {
		this.type = pizzaType;
		this.size = pizzaSize;
		this.crust = "original";
		this.toppings = [];
	}

	bake() {
		console.log(`Baking a ${this.size} ${this.type} ${this.crust} crust pizza.`);
	}

// get pizzaCrust() {

// return this.crust;

// }

// set pizzaCrust(pizzaCrust) {

// this.crust = pizzaCrust;

// }

  
	getCrust() {
		return this.crust;
	}

	setCrust(crust) {
		this.crust = crust;
	}

	getToppings() {
		return this.toppings;
	}

	setToppings(topping) {
		this.toppings.push(topping);
	}
}

  

const myPizza = new Pizza("farmhouse" , "large");

myPizza.setCrust("thin");
myPizza.bake();
myPizza.setToppings("sausage");
myPizza.setToppings("olives");

console.log(myPizza.getToppings());
```

#### Private and Public Fields

```js
// class Pizza {

// constructor(pizzaSize) {

// this._size = pizzaSize; // use _ to make property private

// this._crust = "original";

// }

  

// getCrust() {

// return this._crust;

// }

// setCrust(crust) {

// this._crust = crust;

// }

// }

  

// const pizza = new Pizza("small");

// console.log(pizza.size); // undefined

  

// Use # to create a field private

  

class Pizza {

	crust = "original"; // public field
	#sauce = "traditional"; // private field
	#size;
	
	// both types of field should be declared above the constructor.
	constructor(pizzaSize) {
		this.#size = pizzaSize;
	}
	
	getCrust() {
		return this.crust;
	}
	
	setCrust(crust) {
		this.crust = crust;
	}
	
	hereYouGo() {
		console.log(`Here's your ${this.crust} ${this.#sauce} sauce ${this.#size} pizza.`);
	}

}


const pizza = new Pizza("large");

pizza.hereYouGo();

console.log(pizza.getCrust()); // original
console.log(pizza.crust); // original
console.log(pizza.sauce); // undefined
```

#### Inheritance

```js
class Pizza {
	constructor(pizzaSize) {
		this.size = pizzaSize;
		this.crust = "original";
	}

	getCrust() {
		return this.crust;
	}
	
	setCrust(crust) {
		this.crust = crust;
	}
}

class SpecialityPizza extends Pizza {

	constructor(pizzaSize){
		super(pizzaSize);
		this.type = "The Works";
	}

	slice() {
		console.log(`Our ${this.type} ${this.size} pizza has 8 slices.`);
	}
}

const mySpecialty = new SpecialityPizza("medium");
mySpecialty.slice();
```

#### JSON

Refer to MDN docs:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON

```js
const myObj = {
	name : "Dave",
	hobbies : ["eat", "sleep", "code"],
	hello : function () {
		console.log("Hello!");
	}
};

  
console.log(myObj);
console.log(myObj.name);

myObj.hello();

console.log(typeof myObj);
  
// convert object into json.
const sendJSON = JSON.stringify(myObj);
console.log(sendJSON);

console.log(typeof sendJSON);
console.log(sendJSON.name);

// convert json into object.
const receiveJSON = JSON.parse(sendJSON);
console.log(receiveJSON);

console.log(typeof receiveJSON);
```

#### New operator

Refer to MDN docs - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new

##### Function

```js
function Car(color) {
  if (!new.target) {
    // Called as function.
    return `${color} car`;
  }
  // Called with new.
  this.color = color;
}

const a = Car("red"); // a is "red car"
const b = new Car("red"); // b is `Car { color: "red" }`
```

##### Class

```js
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
}

const p = new Person("Caroline");
p.greet(); // Hello, my name is Caroline
```
