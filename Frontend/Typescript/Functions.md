interface Guitarist {

name? : string,

active: boolean,

albums : (string | number)[]

}

  

// Type Aliases

type stringOrNumber = string | number

  

type stringOrNumberArray = (string | number)[]

  

type GuitaristType = {

name? : string,

active : boolean,

albums : stringOrNumberArray

}

  

type userId = stringOrNumber;

  

//interface postId = userId; This doesn't work. 'userId' only refers to a type, but is being used as a value here.ts(2693)

  

// Literal Types

let myName : 'Dave' // Here 'Dave' is a type and it is working as a type.

  

//myName = "Sanjay";

let userName : 'Dave' | 'John' | 'Amy'

userName = 'Dave';

  

// functions with return type

const add = (a : number, b : number) : number => {

return a + b;

}

  

// void return type and it is a side effect

const logMsg = (message : any) => {

console.log(message);

}

  

logMsg('Hello!');

logMsg(add(2,3));

  

let subtract = function (c : number, d : number): number {

return c - d;

}

  

// Define a type for function using type and interface.

  

//type mathFn = (a : number, b : number) => number;

interface mathFn {

(a : number, b : number) : number

}

  

let multiply: mathFn = function (c,d) {

return c * d

}

  

logMsg(multiply(2, 3));

  

// optional parameters

const addAll = (a : number, b: number, c ?: number) : number => {

return typeof c !== 'undefined' ? a + b + c : a + b;

}

  

// Default Param Value

const sumAll = (a : number = 10, b: number, c: number = 2) : number => a + b + c;

  

logMsg(addAll(2, 3 , 5));

logMsg(addAll(2, 3));

  

logMsg(sumAll(2, 3, 8));

logMsg(sumAll(2, 3));

  

logMsg(sumAll(undefined, 3));

  

// You cannot define the type with the default values.

  

// Rest Parameters.It should be declared as the last parameter.

const total = (a : number, ...nums : number[]) : number => a + nums.reduce((prev , curr) => prev + curr);

  

logMsg(total(1,2,3,4));

  

// never type, when you explicitly throw an error, it returns the never type.

const createError = (errMsg : string) : never => {

throw new Error(errMsg);

}

  

// If this the infite loop then the function return type will be never. However, if it breaks in between then the return type will be void.

const infinite = () => {

let i : number = 1;

while(true) {

i++;

if (i > 100) break;

}

}

  

// custom type guard

const isNumber = (value : any) : boolean => {

return typeof value === 'number' ? true : false;

}

  

// use of the never type.

const numberOrString = (value : number | string) : string => {

if(typeof value === 'string') return 'string'

if(isNumber(value)) return 'number'

return createError('This should never happen!');

}