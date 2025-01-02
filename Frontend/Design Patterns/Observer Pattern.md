
Use observables to notify subscribers when an event occurs.

With the observer pattern, we have:

1. An observable object, which can be observed by subscribers in order to notify them.
2. Subscribers, which can subscribe to and get notified by the observable object.

For example, we can add the logger as a subscriber to the observable.

![[20241231142103.png]]

When the notify method is invoked on the observable, all subscribers get invoked and (optionally) pass the data from the notifier to them.

![[20241231142308.png]]

#### Implementation

We can export a singleton Observer object, which contains a notify, subscribe, and unsubscribe method.

```js
const observers = [];

export default Object.freeze({
  notify: (data) => observers.forEach((observer) => observer(data)),
  subscribe: (func) => observers.push(func),
  unsubscribe: (func) => {
    [...observers].forEach((observer, index) => {
      if (observer === func) {
        observers.splice(index, 1);
      }
    });
  },
});
```

We can use this observable throughout the entire application, making it possible to subscribe functions to the observable.

```js
import Observable from './observable';

function logger(data) {
	console.log(`${Date.now()} ${data}`);
}

Observable.subscribe(logger);
```

and notify all subscribers based on certain events.

```js
import Observable from "./observable";

document.getElementById("my-button").addEventListener("click", () => {
  Observable.notify("Clicked!"); // Notifies all subscribed observers
});
```

#### Example 1

analytics.js

```js
import Observer from './observer.js';

export function sendToGoogleAnalytics(data) {
	console.log('Sent to Google analytics: ', data);
}

export function sendToCustomAnalytics(data) {
	console.log('Sent to custom analytics: ', data);
}

export function sendToEmail(data) {
	console.log('Sent to email: ', data);
}

Observer.subscribe(sendToGoogleAnalytics);
Observer.subscribe(sendToCustomAnalytics);
Observer.subscribe(sendToEmail);
```

index.js

```js
import './analytics.js';
import Observer from './observer.js';

Observer.notify('✨ New data ✨');

setTimeout(() => {
	Observer.notify('✨ New data after timeout ✨');
}, 1000);
```

##### Output

```
Sent to Google analytics:  ✨ New data ✨
Sent to custom analytics:  ✨ New data ✨
Sent to email:  ✨ New data ✨
Sent to Google analytics:  ✨ New data after timeout ✨
Sent to custom analytics:  ✨ New data after timeout ✨
Sent to email:  ✨ New data after timeout ✨
```

#### Tradeoffs

#### Cons

**Separation of Concerns**: The observer objects aren't tightly coupled to the observable object, and can be (de)coupled at any time. The observable object is responsible for monitoring the events, while the observers simply handle the received data.

#### Pros

**Decreased performance**: Notifying all subscribers might take a significant amount of time if the observer handling becomes too complex, or if there are too many subscribers to notify.

#### Exercise

The two buttons in our application both send events to a fake analytics provider.

#### Challenge

Create an observer to which the buttons can subscribe. When a user clicks on a button, this event should get sent to the fake analytics provider.

#### analytics.js 

```js
import Observer from './observer';

export function sendToGoogleAnalytics(data) {
	console.log('Sent to Google analytics: ', data);
}

export function sendToCustomAnalytics(data) {
	console.log('Sent to custom analytics: ', data);
}

export function sendToEmail(data) {
	console.log('Sent to email: ', data);
}


Observer.subscribe(sendToGoogleAnalytics);
Observer.subscribe(sendToCustomAnalytics);
Observer.subscribe(sendToEmail);
```

#### observer.js

```js
const observers = [];

export default Object.freeze({
	notify: (data) => observers.forEach((observer) => observer(data)),
	subscribe: (func) => observers.push(func),
	unsubscribe: (func) => {
		[...observers].forEach((observer, index) => {
			if (observer === func) {
				observers.splice(index, 1);
			}
		});
	},
});
```

#### index.js

```js
import './style.css';
import Observer from './observer.js';

const pinkBtn = document.getElementById('pink-btn');
const blueBtn = document.getElementById('blue-btn');

pinkBtn.addEventListener('click', () => {
	const data = '🎀 Click on pink button! 🎀';
	Observer.notify(data);
});

blueBtn.addEventListener('click', () => {
	const data = '🦋 Click on blue button! 🦋';
	Observer.notify(data);
});
```

#### Output

```
- Sent to Google analytics:
- 🎀 Click on pink button! 🎀

- Sent to custom analytics:
- 🎀 Click on pink button! 🎀

- Sent to email:
- 🎀 Click on pink button! 🎀
    
- Sent to Google analytics:
- 🦋 Click on blue button! 🦋

- Sent to custom analytics:
- 🦋 Click on blue button! 🦋
    
- Sent to email:
- 🦋 Click on blue button! 🦋
```



