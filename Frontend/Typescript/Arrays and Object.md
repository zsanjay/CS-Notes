
let stringArr = ['one', 'hey', 'Dave']

  

let guitars = ['Strat', 'Les Paul', 5150];

  

let mixData = ['EVH', 1984, true];

  

stringArr[0] = 'John'

stringArr.push('hey')

  

guitars[0] = 1984

guitars.unshift('Jim');

  

//stringArr = guitars // It doesn't work you can't assign union type to string array.

  

guitars = stringArr; // But you can assign string type to union which is having string type.

mixData = guitars // same with this.

  

let test = [];

let bands: string[] = []

bands.push('Van Halan');

  

// Tuple - It is more stricter as compared to array

let myTuple : [string, number , boolean] = ['Dave', 42, true];

  

let mixed = ['John', 1, false];

  

//myTuple = mixed // Type '(string | number | boolean)[]' is not assignable to type '[string, number, boolean]'.Target requires 3 element(s) but source may have fewer.ts(2322)

mixed = myTuple // This will work

  

//myTuple[3] = 42; // Type '42' is not assignable to type 'undefined'.ts(2322)

  

//myTuple[2] = 42 // Type 'number' is not assignable to type 'boolean'.ts(2322)

  

myTuple[1] = 42 // This will work.

  
  

// Objects

let myObj : object;

myObj = [];

console.log(typeof myObj);

myObj = bands;

myObj = {};

  

const exampleObj = {

prop1 : 'Dave',

prop2 : true,

}

  

//exampleObj.prop1 = 23; - Type 'number' is not assignable to type 'string'.ts(2322)

exampleObj.prop1 = 'John';

  
  

interface Guitarist {

name? : string,

active: boolean,

albums : (string | number)[]

}

  

type GuitaristType = {

name : string,

active : boolean,

albums : (string | number)[]

}

  

let evh : GuitaristType | Guitarist = {

name : 'Eddie',

active : false,

albums : [1984, 5150, 'OU812']

}

  
  

let jp : Guitarist = {

name : 'Jimmy',

active : true,

albums : ['I', 'II', 'IV']

}

  
  
  

evh = jp // This works because both have the same type.

//evh = jp1 // This doesn't work.

  

// Optional(?) Property

  

interface Guitarist1 {

name : string,

active?: boolean,

albums : (string | number)[]

}

  
  

let jp1 : Guitarist1 = {

name : 'Jimmy',

albums : ['I', 'II', 'IV']

}

  

const greetGuitarist = (guitarist : Guitarist) => {

if(guitarist.name) {

return `Hello ${guitarist.name?.toUpperCase()}!`;

}

return 'Hello!';

}

  

console.log(greetGuitarist(jp));

  
  

// type vs interface

  

// Enums - Unlike most typescript features, Enums are not a type-level addition to JavaScript but something added to the language and runtime.

  

enum Grade {

U = 1, D, C, B, A

}

  

console.log(Grade.U);