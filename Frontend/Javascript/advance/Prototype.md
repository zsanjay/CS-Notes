
```js
// Prototypal Inheritance and the Prototype Chain

  

// ES6 introduced classes which is the modern way to contruct objects

  

// Prototypal Inheritance and the Prototype Chain are:

// 1) "under the hood", (ES6 Classes are considered "syntactic sugar")

// 2) often in interview questions,

// 3) and are useful to understand.

  

// Object literals

  

const person = {

alive : true

}

  

const musician = {

plays : true

}

  

console.log(musician.plays);

console.log(musician.alive);

  

// Musician is a child of Person - Inheritance

// __proto__ defines the parent reference.

// This is the old way

//musician.__proto__ = person;

  

// New way to set proto

Object.setPrototypeOf(musician, person);

console.log(Object.getPrototypeOf(musician));

console.log(musician.__proto__);

console.log(Object.getPrototypeOf(musician) === musician.__proto__);

  

console.log(musician.alive);

  

console.log(musician);

  

// Extending the prototype chain => general to specific to more specific.

  

const guitarist = {

strings: 6,

__proto__: musician

}

  

console.log(guitarist.plays);

console.log(guitarist.alive);

console.log(guitarist.strings);

console.log(guitarist);

  

// No circular references allowed (person.__proto__ can't be guitarist)

// The __proto__ value must be an object or null.

// An object can only directly inherit from one object

  

// object with getter and setter methods

const car = {

doors : 2,

seats : "vinyl",

get seatMaterial() {

return this.seats;

},

set seatMaterial(material) {

this.seats = material;

}

}

  

const luxuryCar = {};

Object.setPrototypeOf(luxuryCar , car);

luxuryCar.seatMaterial = "leather";

console.log(luxuryCar);

console.log(luxuryCar.doors);

console.log(car);

  

// Walking up the chain - props and methods are not copied

console.log(luxuryCar.valueOf());

  

// Getting the Keys of an object

console.log(Object.keys(luxuryCar));

  

// loop through each object key

Object.keys(luxuryCar).forEach(key => {

console.log(key);

});

  

// But a for.. in loop includes inherited props

for(let key in luxuryCar) {

console.log(key);

}

  

// object constructors

function Animal(species) {

this.species = species;

this.eats = true;

}

  

Animal.prototype.walks = function() {

return `A ${this.species} is walking.`;

}

  

const Bear = new Animal("bear");

  

console.log(Bear.species);

console.log(Bear.walks());

  

console.log(Bear.__proto__);

console.log(Bear.__proto__ === Animal.prototype);

console.log(Animal.prototype);

console.log(Bear);

  

console.clear();

  

// Now an ES6 Classes example of inheritance

  

class Vehicle {

constructor() {

this.wheels = 4,

this.motorized = true

}

  

ready() {

return "Ready to go!";

}

}

  

class Motorcycle extends Vehicle {

constructor() {

super();

this.wheels = 2

}

  

wheelie() {

return "On one wheel now!";

}

}

  

const myBike = new Motorcycle();

console.log(myBike);

console.log(myBike.wheels);

console.log(myBike.ready());

console.log(myBike.wheelie());

  

const myTruck = new Vehicle();

console.log(myTruck);
```

### References

https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain

https://www.youtube.com/watch?v=mQ4oCgcgHOA&list=PL0Zuz27SZ-6N3bG4YZhkrCL3ZmDcLTuGd&index=2