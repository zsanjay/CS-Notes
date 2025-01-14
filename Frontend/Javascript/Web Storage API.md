
The **Web Storage API** provides mechanisms by which browsers can store key/value pairs, in a much more intuitive fashion than using [cookies](https://developer.mozilla.org/en-US/docs/Glossary/Cookie).

## [Concepts and usage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API#concepts_and_usage)

The two mechanisms within Web Storage are as follows:

- `sessionStorage` maintains a separate storage area for each given [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin) that's available for the duration of the page session (as long as the browser tab is open, including page reloads and restores).
    
- `localStorage` does the same thing, but persists even when the browser is closed and reopened.

#### Web Storage API

- Not part of the DOM - refers to the Window API
- Available to JS via the global variable: window
- We do not have to type window. It is implied.
	window.alert(window.location);
	alert(location);

####  sessionStorage

```js
const myObject = {
	name : "Sanjay",
	logName : function() {
		console.log(this.name);
	}
}

const myArray = ["eat", "sleep", "code"];
sessionStorage.setItem("mySessionStore" , JSON.stringify(myArray));
let value = JSON.parse(sessionStorage.getItem("mySessionStore"));
console.log(value); // [ "eat", "sleep", "code" ]
```

#### localStorage

```js

localStorage.setItem("mylocalStore" , JSON.stringify(myArray));
localStorage.removeItem("mylocalStore");
localStorage.clear();

const key = localStorage.key(0);
value = JSON.parse(localStorage.getItem(key));
console.log(key);
console.log(localStorage.length);
```

### References

https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API

https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage

https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage