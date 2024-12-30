
| Character classes         |                                |
| ------------------------- | ------------------------------ |
| .                         | any character except newline   |
| \w\d\s                    | word, digit, whitespace        |
| \W\D\S                    | not word, digit, whitespace    |
| [abc]                     | any of a, b, or c              |
| [^abc]                    | not a, b, or c                 |
| [a-g]                     | character between a & g        |
| Anchors                   |                                |
| ^abc$                     | start / end of the string      |
| \b\B                      | word, not-word boundary        |
| Escaped characters        |                                |
| \.\*\\                    | escaped special characters     |
| \t\n\r                    | tab, linefeed, carriage return |
| Groups & Lookaround       |                                |
| (abc)                     | capture group                  |
| \1                        | backreference to group #1      |
| (?:abc)                   | non-capturing group            |
| (?=abc)                   | positive lookahead             |
| (?!abc)                   | negative lookahead             |
| Quantifiers & Alternation |                                |
| a*a+a?                    | 0 or more, 1 or more, 0 or 1   |
| a{5}a{2,}                 | exactly five, two or more      |
| a{1,3}                    | between one & three            |
| a+?a{2,}?                 | match as few as possible       |
| ab\|cd                    | match ab or cd                 |

```html

<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8" />

<meta name="viewport" content="width=device-width, initial-scale=1.0" />

<title>JS Regex Tutorial</title>

<link href="css/regex.css" rel="stylesheet" />

<script defer src="js/regex.js"></script>

</head>

<body>

<main class="flexCenter">

  

<section class="phoneView flexCenter">

<form id="phoneForm" class="phoneForm flexCenter">

<label for="phoneNum">Please enter your phone number:</label>

<input

id="phoneNum"

class="phoneNum"

type="text"

autocomplete="off"

/>

<p class="phoneFormat">Examples:

(555)555-5555 or 123-456-7899

</p>

</form>

</section>

  
  
  

<section class="textView flexCenter">

<form id="textForm" class="textForm flexCenter">

<label for="textEntry">Please enter some text:</label>

<input

id="textEntry"

class="textEntry"

type="text"

autocomplete="off"

/>

</form>

</section>

</main>

</body>

</html>
```

```js

document.getElementById("phoneNum").addEventListener("input", (event) => {

const regex = /^\(?(\d{3})\)?[-. ]?(\d{3})[-. ]?(\d{4})$/g;

const input = document.getElementById("phoneNum");

const format = document.querySelector(".phoneFormat");

const phone = input.value;

const found = regex.test(phone);

if(!found && phone.length) {

input.classList.add("invalid");

format.classList.add("block");

} else {

input.classList.remove("invalid");

format.classList.remove("block");

}

});

  

document.getElementById("phoneForm").addEventListener("submit", (event) => {

event.preventDefault();

const input = document.getElementById("phoneNum");

const regex = /[()-. ]/g;

const savedPhoneNum = input.value.replaceAll(regex, "");

console.log(savedPhoneNum);

});

  
  

document.getElementById("textForm").addEventListener("submit", (event) => {

event.preventDefault();

const input = document.getElementById("textEntry");

const regex = / {2,}/g;

const newText = input.value.replaceAll(regex, " ").trim();

console.log(newText);

const encodedInputText = encodeURI(input.value);

const encodedCleanText = encodeURI(newText);

console.log(encodedInputText);

console.log(encodedCleanText);

});
```

```css

* {

margin: 0;

padding: 0;

box-sizing: border-box;

}

.flexCenter {

display: flex;

flex-direction: column;

justify-content: center;

align-items: center;

}

html,

body {

width: 100vw;

height: 100vh;

font-family: Arial, Helvetica, sans-serif;

font-size: 22px;

}

@media only screen and (min-width: 768px) {

html,

body {

font-size: 44px;

}

}

section {

width: 100vw;

height: 100vh;

}

label {

text-align: center;

margin-bottom: 0.5rem;

}

input {

font-size: 1rem;

text-align: center;

padding: 0.5rem;

margin-bottom: 0.5rem;

width: 90vw;

letter-spacing: 5px;

border-radius: 25px;

outline: none;

}

input:hover,

input:focus {

box-shadow: 2px 2px 10px #424242;

}

@media only screen and (min-width: 1024px) {

input {

width: 50vw;

}

}

.phoneFormat {

display: none;

text-align: center;

font-size: 0.75rem;

}

.block {

display: block;

}

.invalid {

color: red;

}
```

### References

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions

https://regexr.com

https://regex101.com

https://www.youtube.com/watch?v=3l08sBKOSCs&list=PL0Zuz27SZ-6Oi6xNtL_fwCrwpuqylMsgT&index=27

https://github.com/gitdagray/regex_js_examples