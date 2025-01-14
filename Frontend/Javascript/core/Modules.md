
Refer to docs :
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules 

```js
<script type="modules" src="js/web_storage_api.js"></script>
```

type = "modules" does two things:

1. Includes defer
2. Applies 'use strict'

#### Import and export

##### Export 

```js
export function playGuitar() {
	return "Playing guitar!";
}

export const shredding = () => {
	return "Shredding some licks!";
}

export const plucking = () => {
	return "Plucking the strings....";
}

// export default playGuitar;
// export { shredding , plucking};
```


```js
export default class User {
	constructor(email, name) {
		this._id = email; // _ means private
		this._name = name; // _ means private
	}
	greeting() {
		return `Hi, my name is ${this._name}.`;
	}
}
```

##### Import 

```js
import playGuitar from "./guitars.js";
import { shredding as shred, plucking as fingerpicking } from "./guitars.js";

console.log(playGuitar());
console.log(shred());
console.log(fingerpicking());
```

```js
import * as Guitars from "./guitars.js";
import User from './user.js';

console.log(Guitars.playGuitar());
console.log(Guitars.shredding());
console.log(Guitars.plucking());

const me = new User("email@gmail.com" , "Sanjay");
console.log(me);
console.log(me.greeting());
```

