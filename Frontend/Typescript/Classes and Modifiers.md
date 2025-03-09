```ts
// class without visibility modifier

class Coder {

name: string;

music: string;

age: number;

lang: string;

  

constructor(name: string, music: string, age: number, lang: string) {

this.name = name;

this.music = music;

this.age = age;

this.lang = lang;

}

}

  

// class with visibility modifier

class Coder1 {

secondLang!: string;

  

constructor(

public readonly name: string,

public music: string,

private age: number,

protected lang: string = "Typescript"

) {}

  

public getAge() {

return `Hello, I'm ${this.age}`;

}

}

  

const Dave = new Coder1("Dave", "Rock", 42);

console.log(Dave.getAge());

// console.log(Dave.age);

// console.log(Dave.lang);

  

class WebDev extends Coder1 {

constructor(

public computer: string,

name: string,

music: string,

age: number

) {

super(name, music, age)

this.computer = computer;

}

  

public getLang() {

return `I write ${this.lang}` // protected members of the parent class can only be accessible within the subclass not outside

}

}

  

const Sara = new WebDev('Mac', 'Sara', 'Lofi', 25);

console.log(Sara.getLang());

//console.log(Sara.age); // Private not accessible

//console.log(Sara.lang); // Protected not accessible

  

interface Musician {

name : string,

instrument : string,

play(action : string) : string

}

  

// The property and function name and type should match while implementing the interface.

  

class Guitarist implements Musician {

name : string

instrument: string

  

constructor(name : string, instrument : string) {

this.name = name

this.instrument = instrument

}

  

play(action : string) : string {

return `${this.name} ${action} the ${this.instrument}`;

}

}

  

const Page = new Guitarist('Jimmy' , 'guitar');

console.log(Page.play('strums'));

  

/////////////////////////////////

  

class Peeps {

static count : number = 0

  

static getCount(): number {

return Peeps.count;

}

  

public id : number

  

constructor(public name : string) {

this.id = ++Peeps.count;

}

}

  
  

const John = new Peeps('John');

const Steve = new Peeps('Steve');

const Amy = new Peeps('Amy');

  

console.log(Amy.id);

console.log(Steve.id);

console.log(John.id);

console.log(Peeps.count);

  

/////////////////////////////////

  

class Bands {

private dataState : string[]

  

constructor() {

this.dataState = []

}

  

public get data() : string[] {

return this.dataState

}

  

public set data(value : string[]) {

if(Array.isArray(value) && value.every(el => typeof el === 'string')) {

this.dataState = value

return

} else throw new Error('Param is not an array of strings');

}

}

  

const myBands = new Bands()

myBands.data = ['Neil Young', 'Led Zep']

console.log(myBands.data);

myBands.data = [...myBands.data, 'ZZ Top']

console.log(myBands.data)

//myBands.data = ['Van Halen', 4324]
```
