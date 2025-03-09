### How to remove unnecessary Effects.

There are two common cases in which you don’t need Effects:

1. **You don’t need Effects to transform data for rendering.** For example, let’s say you want to filter a list before displaying it. You might feel tempted to write an Effect that updates a state variable when the list changes. However, this is inefficient. When you update the state, React will first call your component functions to calculate what should be on the screen. Then React will [“commit”](https://react.dev/learn/render-and-commit) these changes to the DOM, updating the screen. Then React will run your Effects. If your Effect _also_ immediately updates the state, this restarts the whole process from scratch! To avoid the unnecessary render passes, transform all the data at the top level of your components. That code will automatically re-run whenever your props or state change.
2. **You don’t need Effects to handle user events.** For example, let’s say you want to send an `/api/buy` POST request and show a notification when the user buys a product. In the Buy button click event handler, you know exactly what happened. By the time an Effect runs, you don’t know _what_ the user did (for example, which button was clicked). This is why you’ll usually handle user events in the corresponding event handlers.

You _do_ need Effects to [synchronize](https://react.dev/learn/synchronizing-with-effects#what-are-effects-and-how-are-they-different-from-events) with external systems. For example, you can write an Effect that keeps a jQuery widget synchronized with the React state. You can also fetch data with Effects: for example, you can synchronize the search results with the current search query. Keep in mind that modern [frameworks](https://react.dev/learn/start-a-new-react-project#production-grade-react-frameworks) provide more efficient built-in data fetching mechanisms than writing Effects directly in your components.

### Updating state based on props or state 

Avoid: redundant state and unnecessary Effect  

```jsx
function Form() {  
	const [firstName, setFirstName] = useState('Taylor');  
	const [lastName, setLastName] = useState('Swift');  

	// 🔴 Avoid: redundant state and unnecessary Effect  
	const [fullName, setFullName] = useState('');  
	useEffect(() => {  
		setFullName(firstName + ' ' + lastName);  
	}, [firstName, lastName]);  
	// ...  
}
```

Good: calculated during rendering  

```jsx
function Form() {  
	const [firstName, setFirstName] = useState('Taylor');  
	const [lastName, setLastName] = useState('Swift');  
	// ✅ Good: calculated during rendering  
	const fullName = firstName + ' ' + lastName;  
	// ...  
}
```

### Caching expensive calculations

```jsx
function TodoList({ todos, filter }) {  
	const [newTodo, setNewTodo] = useState('');  

	// 🔴 Avoid: redundant state and unnecessary Effect  
	const [visibleTodos, setVisibleTodos] = useState([]);  
	useEffect(() => {  
		setVisibleTodos(getFilteredTodos(todos, filter));  
	}, [todos, filter]);  
	// ...  
}
```

Like in the earlier example, this is both unnecessary and inefficient. First, remove the state and the Effect:

```jsx
function TodoList({ todos, filter }) {  
	const [newTodo, setNewTodo] = useState('');  
	// ✅ This is fine if getFilteredTodos() is not slow.  
	const visibleTodos = getFilteredTodos(todos, filter);  
	// ...  
}
```

Usually, this code is fine! But maybe `getFilteredTodos()` is slow or you have a lot of `todos`. In that case you don’t want to recalculate `getFilteredTodos()` if some unrelated state variable like `newTodo` has changed.

You can cache (or [“memoize”](https://en.wikipedia.org/wiki/Memoization)) an expensive calculation by wrapping it in a [`useMemo`](https://react.dev/reference/react/useMemo) Hook:
## useMemo Hook

`useMemo` is a React Hook that lets you cache the result of a calculation between re-renders.

```jsx
import { useMemo, useState } from 'react';  

function TodoList({ todos, filter }) {  
	const [newTodo, setNewTodo] = useState('');  
	// ✅ Does not re-run getFilteredTodos() unless todos or filter change  
	const visibleTodos = useMemo(() => getFilteredTodos(todos, filter), [todos, filter]);  
	// ...  
}
```

**This tells React that you don’t want the inner function to re-run unless either `todos` or `filter` have changed.** React will remember the return value of `getFilteredTodos()` during the initial render. During the next renders, it will check if `todos` or `filter` are different. If they’re the same as last time, `useMemo` will return the last result it has stored. But if they are different, React will call the inner function again (and store its result).

The function you wrap in [`useMemo`](https://react.dev/reference/react/useMemo) runs during rendering, so this only works for [pure calculations.](https://react.dev/learn/keeping-components-pure)

#### How to tell if a calculation is expensive?

In general, unless you’re creating or looping over thousands of objects, it’s probably not expensive. If you want to get more confidence, you can add a console log to measure the time spent in a piece of code:

```js
console.time('filter array');
const visibleTodos = getFilteredTodos(todos, filter);
console.timeEnd('filter array');
```

Perform the interaction you’re measuring (for example, typing into the input). You will then see logs like `filter array: 0.15ms` in your console. If the overall logged time adds up to a significant amount (say, `1ms` or more), it might make sense to memoize that calculation. As an experiment, you can then wrap the calculation in `useMemo` to verify whether the total logged time has decreased for that interaction or not:

```js
console.time('filter array');
const visibleTodos = useMemo(() => { return getFilteredTodos(todos, filter);
// Skipped if todos and filter haven't changed
}, [todos, filter]);
console.timeEnd('filter array');
```

`useMemo` won’t make the _first_ render faster. It only helps you skip unnecessary work on updates.

Keep in mind that your machine is probably faster than your users’ so it’s a good idea to test the performance with an artificial slowdown. For example, Chrome offers a [CPU Throttling](https://developer.chrome.com/blog/new-in-devtools-61/#throttling) option for this.

Also note that measuring performance in development will not give you the most accurate results. (For example, when [Strict Mode](https://react.dev/reference/react/StrictMode) is on, you will see each component render twice rather than once.) To get the most accurate timings, build your app for production and test it on a device like your users have.

### Resetting all state when a prop changes

This `ProfilePage` component receives a `userId` prop. The page contains a comment input, and you use a `comment` state variable to hold its value. One day, you notice a problem: when you navigate from one profile to another, the `comment` state does not get reset. As a result, it’s easy to accidentally post a comment on a wrong user’s profile. To fix the issue, you want to clear out the `comment` state variable whenever the `userId` changes:

```jsx
export default function ProfilePage({ userId }) {  
	const [comment, setComment] = useState('');  

	// 🔴 Avoid: Resetting state on prop change in an Effect  
	useEffect(() => {  
		setComment('');  
	}, [userId]);  
	// ...  
}
```

Instead, you can tell React that each user’s profile is conceptually a _different_ profile by giving it an explicit key. Split your component in two and pass a `key` attribute from the outer component to the inner one:

```jsx
export default function ProfilePage({ userId }) {  
	return (  
		<Profile  
			userId={userId}  
			key={userId}  
		/>  
	);  
}  

function Profile({ userId }) {  
	// ✅ This and any other state below will reset on key change automatically  
	const [comment, setComment] = useState('');  
	// ...  
}
```

Normally, React preserves the state when the same component is rendered in the same spot. **By passing `userId` as a `key` to the `Profile` component, you’re asking React to treat two `Profile` components with different `userId` as two different components that should not share any state.** Whenever the key (which you’ve set to `userId`) changes, React will recreate the DOM and [reset the state](https://react.dev/learn/preserving-and-resetting-state#option-2-resetting-state-with-a-key) of the `Profile` component and all of its children. Now the `comment` field will clear out automatically when navigating between profiles.

Note that in this example, only the outer `ProfilePage` component is exported and visible to other files in the project. Components rendering `ProfilePage` don’t need to pass the key to it: they pass `userId` as a regular prop. The fact `ProfilePage` passes it as a `key` to the inner `Profile` component is an implementation detail.

### Adjusting some state when a prop changes

Sometimes, you might want to reset or adjust a part of the state on a prop change, but not all of it.

This `List` component receives a list of `items` as a prop, and maintains the selected item in the `selection` state variable. You want to reset the `selection` to `null` whenever the `items` prop receives a different array:

```jsx
function List({ items }) {  
	const [isReverse, setIsReverse] = useState(false);  
	const [selection, setSelection] = useState(null);  

	// 🔴 Avoid: Adjusting state on prop change in an Effect  
	useEffect(() => {  
		setSelection(null);  
	}, [items]);  
	// ...  
}
```

This, too, is not ideal. Every time the `items` change, the `List` and its child components will render with a stale `selection` value at first. Then React will update the DOM and run the Effects. Finally, the `setSelection(null)` call will cause another re-render of the `List` and its child components, restarting this whole process again.

Start by deleting the Effect. Instead, adjust the state directly during rendering:

```jsx
function List({ items }) {  
	const [isReverse, setIsReverse] = useState(false);  
	const [selection, setSelection] = useState(null);  

	// Better: Adjust the state while rendering  
	const [prevItems, setPrevItems] = useState(items);  
	if (items !== prevItems) {  
		setPrevItems(items);  
		setSelection(null);  
	}  
	// ...  
}
```


Pending.......


