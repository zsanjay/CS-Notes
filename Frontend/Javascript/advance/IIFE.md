```js
// IIFE (Immediately Invoked Function Expression)
const privateCounter = (() => {
	let count = 0;
	console.log(`initial value: ${count}`);
	return () => { count += 1; console.log(count) }
})();

privateCounter();
privateCounter();
privateCounter();

const credits = ((num) => {
	let credits = num;
	console.log(`initial credits value: ${credits}`);
	return () => {
		credits -= 1;
		if(credits > 0) {
			console.log(`playing game, ${credits} credits(s) remaining`);
		} else {
			console.log("not enough credits");
		}
	}
})(3);

credits();
credits();
credits();
```

### References

https://developer.mozilla.org/en-US/docs/Glossary/IIFE