
Type Assertion or Type Casting 

type One = string

type Two = string | number

type Three = 'hello'


// convert to more or less specific

let a : One = 'hello'

let b = a as Two // less specific

let c = a as Three // more specific

  

const addOrConcat = (a : number , b: number, c : 'add' | 'concat') : number | string => c === 'add' ? a + b : '' + a + b;

  

let myVal : string = addOrConcat(2, 2, 'concat') as string;

  

// TS sees no problem - but string is returned

let nextVal : number = addOrConcat(2 , 2, 'concat') as number;

  
  

// unknown type

  

//10 as string // Conversion of type 'number' to type 'string' may be a mistake because neither type sufficiently overlaps with the other.

//If this was intentional, convert the expression to 'unknown' first.ts(2352)

  

(10 as unknown) as string

  
  

// The DOM - Assertions help us to remove null.

const img = document.querySelector('img') as HTMLImageElement // Typescript able to infer the type as - const img: HTMLImageElement.

const imgById = document.querySelector('#imgId') as Element // Typescript able to infer the type as - const imgById: Element.

const myImg = document.getElementById('#img') as HTMLImageElement// TS infer the type as - const myImg: HTMLImageElement.


img.src

myImg.src

```ts
// Original JS code

// const year = document.getElementById("year");

// const thisYear = new Date().getFullYear();

// year?.setAttribute("datetime", thisYear);

// year?.textContent = thisYear;

  

// TS code - 1st Variation

// let year : HTMLElement | null

// year = document.getElementById("year");

// let thisYear : string

// thisYear = new Date().getFullYear().toString();

// if(year) {

// year.setAttribute("datetime", thisYear);

// year.textContent = thisYear;

// }

  

// TS code - 2nd Variation with Assertions

const year = document.getElementById("year") as HTMLSpanElement;

const thisYear : string = new Date().getFullYear().toString();

year.setAttribute("datetime", thisYear);

year.textContent = thisYear;
```



```html
<html lang="en">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Copyright</title>

<style>

body {

margin: 0;

min-height: 100vh;

background-color: #000;

display: grid;

place-content: center;

font-size: 3rem;

color: #fff;

}

</style>

<script src="js/copyright.js" defer></script>

</head>

<body>

<p>Copyright &copy; <span id="year"></span></p>

</body>

</html>
```