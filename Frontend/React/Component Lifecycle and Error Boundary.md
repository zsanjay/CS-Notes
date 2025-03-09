
`Component` is the base class for the React components defined as [JavaScript classes.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) Class components are still supported by React, but we don’t recommend using them in new code.

```jsx
class Greeting extends Component {  
	render() {  
		return <h1>Hello, {this.props.name}!</h1>;  
	}  
}
```

### `Component` 

To define a React component as a class, extend the built-in `Component` class and define a [`render` method:](https://react.dev/reference/react/Component#render)

```jsx
import { Component } from 'react';  

class Greeting extends Component {  
	render() {  
		return <h1>Hello, {this.props.name}!</h1>;  
	}  
}
```

Only the `render` method is required, other methods are optional.


### `context` 

The [context](https://react.dev/learn/passing-data-deeply-with-context) of a class component is available as `this.context`. It is only available if you specify _which_ context you want to receive using [`static contextType`](https://react.dev/reference/react/Component#static-contexttype).

A class component can only read one context at a time.

```jsx
class Button extends Component {  

static contextType = ThemeContext;  

render() {  
	const theme = this.context;  
	const className = 'button-' + theme;  
	
	return (  
		<button className={className}>  
			{this.props.children}  
		</button>  
	);  
  }  
}
```

Reading `this.context` in class components is equivalent to [`useContext`](https://react.dev/reference/react/useContext) in function components.

### `props` 

The props passed to a class component are available as `this.props`.

```jsx
class Greeting extends Component {  
	render() {  
		return <h1>Hello, {this.props.name}!</h1>;  
	}  
}  

<Greeting name="Taylor" />
```

Reading `this.props` in class components is equivalent to [declaring props](https://react.dev/learn/passing-props-to-a-component#step-2-read-props-inside-the-child-component) in function components.

### `state`

The state of a class component is available as `this.state`. The `state` field must be an object. Do not mutate the state directly. If you wish to change the state, call `setState` with the new state.

```jsx
class Counter extends Component {  

	state = {  
		age: 42,  
	};  
	
	handleAgeChange = () => {  
		this.setState({  
			age: this.state.age + 1  
		});  
	};  
	
	render() {  
		return (  
			<>  
				<button onClick={this.handleAgeChange}>  
					Increment age  
				</button>  
				<p>You are {this.state.age}.</p>  
			</>  
		);  
	}  
}
```

Defining `state` in class components is equivalent to calling [`useState`](https://react.dev/reference/react/useState) in function components.

### `constructor(props)`

The [constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor) runs before your class component _mounts_ (gets added to the screen). Typically, a constructor is only used for two purposes in React. It lets you declare state and [bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) your class methods to the class instance:

```jsx
class Counter extends Component {  

constructor(props) {  
	super(props);  
	this.state = { counter: 0 };  
	this.handleClick = this.handleClick.bind(this);  
}  

handleClick() {  
// ...  
}
```

If you use modern JavaScript syntax, constructors are rarely needed. Instead, you can rewrite this code above using the [public class field syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields) which is supported both by modern browsers and tools like [Babel:](https://babeljs.io/)

```jsx
class Counter extends Component {  
	state = { counter: 0 };  
	
	handleClick = () => { 
		// ...  
	}
```

A constructor should not contain any side effects or subscriptions.
#### Parameters

- `props`: The component’s initial props.

#### Returns 

`constructor` should not return anything.

#### Caveats 

- Do not run any side effects or subscriptions in the constructor. Instead, use [`componentDidMount`](https://react.dev/reference/react/Component#componentdidmount) for that.

- Inside a constructor, you need to call `super(props)` before any other statement. If you don’t do that, `this.props` will be `undefined` while the constructor runs, which can be confusing and cause bugs.

- Constructor is the only place where you can assign [`this.state`](https://react.dev/reference/react/Component#state) directly. In all other methods, you need to use [`this.setState()`](https://react.dev/reference/react/Component#setstate) instead. Do not call `setState` in the constructor.

- When you use [server rendering,](https://react.dev/reference/react-dom/server) the constructor will run on the server too, followed by the [`render`](https://react.dev/reference/react/Component#render) method. However, lifecycle methods like `componentDidMount` or `componentWillUnmount` will not run on the server.

- When [Strict Mode](https://react.dev/reference/react/StrictMode) is on, React will call `constructor` twice in development and then throw away one of the instances. This helps you notice the accidental side effects that need to be moved out of the `constructor`.

There is no exact equivalent for `constructor` in function components. To declare state in a function component, call [`useState`.](https://react.dev/reference/react/useState) To avoid recalculating the initial state, [pass a function to `useState`.](https://react.dev/reference/react/useState#avoiding-recreating-the-initial-state)

### `componentDidCatch(error, info)`

If you define `componentDidCatch`, React will call it when some child component (including distant children) throws an error during rendering. This lets you log that error to an error reporting service in production.

Typically, it is used together with [`static getDerivedStateFromError`](https://react.dev/reference/react/Component#static-getderivedstatefromerror) which lets you update state in response to an error and display an error message to the user. A component with these methods is called an _error boundary._

#### Parameters 

- `error`: The error that was thrown. In practice, it will usually be an instance of [`Error`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error) but this is not guaranteed because JavaScript allows to [`throw`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/throw) any value, including strings or even `null`.

- `info`: An object containing additional information about the error. Its `componentStack` field contains a stack trace with the component that threw, as well as the names and source locations of all its parent components. In production, the component names will be minified. If you set up production error reporting, you can decode the component stack using sourcemaps the same way as you would do for regular JavaScript error stacks.


#### Returns 

`componentDidCatch` should not return anything.

#### Caveats

- In the past, it was common to call `setState` inside `componentDidCatch` in order to update the UI and display the fallback error message. This is deprecated in favor of defining [`static getDerivedStateFromError`.](https://react.dev/reference/react/Component#static-getderivedstatefromerror)

- Production and development builds of React slightly differ in the way `componentDidCatch` handles errors. In development, the errors will bubble up to `window`, which means that any `window.onerror` or `window.addEventListener('error', callback)` will intercept the errors that have been caught by `componentDidCatch`. In production, instead, the errors will not bubble up, which means any ancestor error handler will only receive errors not explicitly caught by `componentDidCatch`.


### Catching rendering errors with an error boundary

By default, if your application throws an error during rendering, React will remove its UI from the screen. To prevent this, you can wrap a part of your UI into an _error boundary_. An error boundary is a special component that lets you display some fallback UI instead of the part that crashed—for example, an error message.

To implement an error boundary component, you need to provide [`static getDerivedStateFromError`](https://react.dev/reference/react/Component#static-getderivedstatefromerror) which lets you update state in response to an error and display an error message to the user. You can also optionally implement [`componentDidCatch`](https://react.dev/reference/react/Component#componentdidcatch) to add some extra logic, for example, to log the error to an analytics service.

```jsx
import * as React from 'react';  

  

class ErrorBoundary extends React.Component {  

	constructor(props) {  
		super(props);  
		this.state = { hasError: false };  
	}  

  

static getDerivedStateFromError(error) {  
	// Update state so the next render will show the fallback UI.  
	return { hasError: true };  
}  

  

componentDidCatch(error, info) {  
	logErrorToMyService(  
		error,  
		// Example "componentStack":  
		// in ComponentThatThrows (created by App)  
		// in ErrorBoundary (created by App)  
		// in div (created by App)  
		// in App  
		info.componentStack,  
		
		// Only available in react@canary.  
		// Warning: Owner Stack is not available in production.  
		React.captureOwnerStack(),  
	);  
}  

  

render() {  
	if (this.state.hasError) {  
		// You can render any custom fallback UI  
		 return this.props.fallback;  
	}  
		return this.props.children;  
	}  
}
```

Then you can wrap a part of your component tree with it:

```jsx
<ErrorBoundary fallback={<p>Something went wrong</p>}>  
	<Profile />  
</ErrorBoundary>
```

If `Profile` or its child component throws an error, `ErrorBoundary` will “catch” that error, display a fallback UI with the error message you’ve provided, and send a production error report to your error reporting service.

You don’t need to wrap every component into a separate error boundary. When you think about the [granularity of error boundaries,](https://www.brandondail.com/posts/fault-tolerance-react) consider where it makes sense to display an error message. For example, in a messaging app, it makes sense to place an error boundary around the list of conversations. It also makes sense to place one around every individual message. However, it wouldn’t make sense to place a boundary around every avatar.

### Note:

There is no direct equivalent for `componentDidCatch` in function components yet. If you’d like to avoid creating class components, write a single `ErrorBoundary` component like above and use it throughout your app. Alternatively, you can use the [`react-error-boundary`](https://github.com/bvaughn/react-error-boundary) package which does that for you.

### `componentDidMount()`

If you define the `componentDidMount` method, React will call it when your component is added _(mounted)_ to the screen. This is a common place to start data fetching, set up subscriptions, or manipulate the DOM nodes.

If you implement `componentDidMount`, you usually need to implement other lifecycle methods to avoid bugs. For example, if `componentDidMount` reads some state or props, you also have to implement [`componentDidUpdate`](https://react.dev/reference/react/Component#componentdidupdate) to handle their changes, and [`componentWillUnmount`](https://react.dev/reference/react/Component#componentwillunmount) to clean up whatever `componentDidMount` was doing.

```jsx
class ChatRoom extends Component {  

state = {  
	serverUrl: 'https://localhost:1234'  
};  

componentDidMount() {  
	this.setupConnection();  
}  

componentDidUpdate(prevProps, prevState) {  
	if (this.props.roomId !== prevProps.roomId ||  this.state.serverUrl !== prevState.serverUrl  
	) {  
		this.destroyConnection();  
		this.setupConnection();  
	}  
}  

componentWillUnmount() {  
	this.destroyConnection();  
}  
// ...  
}
```

#### Parameters 

`componentDidMount` does not take any parameters.

#### Returns 

`componentDidMount` should not return anything.

#### Caveats 

- When [Strict Mode](https://react.dev/reference/react/StrictMode) is on, in development React will call `componentDidMount`, then immediately call [`componentWillUnmount`,](https://react.dev/reference/react/Component#componentwillunmount) and then call `componentDidMount` again. This helps you notice if you forgot to implement `componentWillUnmount` or if its logic doesn’t fully “mirror” what `componentDidMount` does.

- Although you may call [`setState`](https://react.dev/reference/react/Component#setstate) immediately in `componentDidMount`, it’s best to avoid that when you can. It will trigger an extra rendering, but it will happen before the browser updates the screen. This guarantees that even though the [`render`](https://react.dev/reference/react/Component#render) will be called twice in this case, the user won’t see the intermediate state. Use this pattern with caution because it often causes performance issues. In most cases, you should be able to assign the initial state in the [`constructor`](https://react.dev/reference/react/Component#constructor) instead. It can, however, be necessary for cases like modals and tooltips when you need to measure a DOM node before rendering something that depends on its size or position.


### Note:

For many use cases, defining `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` together in class components is equivalent to calling [`useEffect`](https://react.dev/reference/react/useEffect) in function components. In the rare cases where it’s important for the code to run before browser paint, [`useLayoutEffect`](https://react.dev/reference/react/useLayoutEffect) is a closer match.

### `componentDidUpdate(prevProps, prevState, snapshot?)` 

If you define the `componentDidUpdate` method, React will call it immediately after your component has been re-rendered with updated props or state. This method is not called for the initial render.

You can use it to manipulate the DOM after an update. This is also a common place to do network requests as long as you compare the current props to previous props (e.g. a network request may not be necessary if the props have not changed). Typically, you’d use it together with [`componentDidMount`](https://react.dev/reference/react/Component#componentdidmount) and [`componentWillUnmount`:](https://react.dev/reference/react/Component#componentwillunmount)

```jsx
class ChatRoom extends Component {  

state = {  
	serverUrl: 'https://localhost:1234'  
};  

componentDidMount() {  
	this.setupConnection();  
}  


componentDidUpdate(prevProps, prevState) {  
	if (  
		this.props.roomId !== prevProps.roomId ||  
		this.state.serverUrl !== prevState.serverUrl  
	) {  
		this.destroyConnection();  
		this.setupConnection();  
	}  
}  

componentWillUnmount() {  
	this.destroyConnection();  
}  

// ...  
}
```

#### Parameters 

- `prevProps`: Props before the update. Compare `prevProps` to [`this.props`](https://react.dev/reference/react/Component#props) to determine what changed.

- `prevState`: State before the update. Compare `prevState` to [`this.state`](https://react.dev/reference/react/Component#state) to determine what changed.

- `snapshot`: If you implemented [`getSnapshotBeforeUpdate`](https://react.dev/reference/react/Component#getsnapshotbeforeupdate), `snapshot` will contain the value you returned from that method. Otherwise, it will be `undefined`.

#### Returns 

`componentDidUpdate` should not return anything.

#### Caveats

- `componentDidUpdate` will not get called if [`shouldComponentUpdate`](https://react.dev/reference/react/Component#shouldcomponentupdate) is defined and returns `false`.

- The logic inside `componentDidUpdate` should usually be wrapped in conditions comparing `this.props` with `prevProps`, and `this.state` with `prevState`. Otherwise, there’s a risk of creating infinite loops.

### Note:

For many use cases, defining `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` together in class components is equivalent to calling [`useEffect`](https://react.dev/reference/react/useEffect) in function components. In the rare cases where it’s important for the code to run before browser paint, [`useLayoutEffect`](https://react.dev/reference/react/useLayoutEffect) is a closer match.

## useLayoutEffect

`useLayoutEffect` can hurt performance. Prefer [`useEffect`](https://react.dev/reference/react/useEffect) when possible.

`useLayoutEffect` is a version of [`useEffect`](https://react.dev/reference/react/useEffect) that fires before the browser repaints the screen.

```jsx
useLayoutEffect(setup, dependencies?)
```

Call `useLayoutEffect` to perform the layout measurements before the browser repaints the screen:

```jsx
import { useState, useRef, useLayoutEffect } from 'react';  

function Tooltip() {  
	const ref = useRef(null);  
	const [tooltipHeight, setTooltipHeight] = useState(0);  

useLayoutEffect(() => {  
	const { height } = ref.current.getBoundingClientRect();  
	setTooltipHeight(height);  
}, []);  
// ...
```


#### Parameters 

- `setup`: The function with your Effect’s logic. Your setup function may also optionally return a _cleanup_ function. Before your component is added to the DOM, React will run your setup function. After every re-render with changed dependencies, React will first run the cleanup function (if you provided it) with the old values, and then run your setup function with the new values. Before your component is removed from the DOM, React will run your cleanup function.

- **optional** `dependencies`: The list of all reactive values referenced inside of the `setup` code. Reactive values include props, state, and all the variables and functions declared directly inside your component body. If your linter is [configured for React](https://react.dev/learn/editor-setup#linting), it will verify that every reactive value is correctly specified as a dependency. The list of dependencies must have a constant number of items and be written inline like `[dep1, dep2, dep3]`. React will compare each dependency with its previous value using the [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) comparison. If you omit this argument, your Effect will re-run after every re-render of the component.

#### Returns 

`useLayoutEffect` returns `undefined`.

#### Caveats 

- `useLayoutEffect` is a Hook, so you can only call it **at the top level of your component** or your own Hooks. You can’t call it inside loops or conditions. If you need that, extract a component and move the Effect there.

- When Strict Mode is on, React will **run one extra development-only setup+cleanup cycle** before the first real setup. This is a stress-test that ensures that your cleanup logic “mirrors” your setup logic and that it stops or undoes whatever the setup is doing. If this causes a problem, [implement the cleanup function.](https://react.dev/learn/synchronizing-with-effects#how-to-handle-the-effect-firing-twice-in-development)

- If some of your dependencies are objects or functions defined inside the component, there is a risk that they will **cause the Effect to re-run more often than needed.** To fix this, remove unnecessary [object](https://react.dev/reference/react/useEffect#removing-unnecessary-object-dependencies) and [function](https://react.dev/reference/react/useEffect#removing-unnecessary-function-dependencies) dependencies. You can also [extract state updates](https://react.dev/reference/react/useEffect#updating-state-based-on-previous-state-from-an-effect) and [non-reactive logic](https://react.dev/reference/react/useEffect#reading-the-latest-props-and-state-from-an-effect) outside of your Effect.

- Effects **only run on the client.** They don’t run during server rendering.

- The code inside `useLayoutEffect` and all state updates scheduled from it **block the browser from repainting the screen.** When used excessively, this makes your app slow. When possible, prefer [`useEffect`.](https://react.dev/reference/react/useEffect)

- If you trigger a state update inside `useLayoutEffect`, React will execute all remaining Effects immediately including `useEffect`.

### `componentWillUnmount() - deprecated`

If you define the `componentWillUnmount` method, React will call it before your component is removed _(unmounted)_ from the screen. This is a common place to cancel data fetching or remove subscriptions.

The logic inside `componentWillUnmount` should “mirror” the logic inside [`componentDidMount`.](https://react.dev/reference/react/Component#componentdidmount) For example, if `componentDidMount` sets up a subscription, `componentWillUnmount` should clean up that subscription. If the cleanup logic in your `componentWillUnmount` reads some props or state, you will usually also need to implement [`componentDidUpdate`](https://react.dev/reference/react/Component#componentdidupdate) to clean up resources (such as subscriptions) corresponding to the old props and state.

```jsx
class ChatRoom extends Component {  

state = {  
	serverUrl: 'https://localhost:1234'  
};  

componentDidMount() {  
	this.setupConnection();  
}  

componentDidUpdate(prevProps, prevState) {  
	if (  
		this.props.roomId !== prevProps.roomId ||  
		this.state.serverUrl !== prevState.serverUrl  
	) {  
		this.destroyConnection();  
		this.setupConnection();  
	}  
}  

componentWillUnmount() {  
	this.destroyConnection();  
}  
// ...  
}
```

#### Parameters 

`componentWillUnmount` does not take any parameters.

#### Returns 

`componentWillUnmount` should not return anything.

#### Caveats 

- When [Strict Mode](https://react.dev/reference/react/StrictMode) is on, in development React will call [`componentDidMount`,](https://react.dev/reference/react/Component#componentdidmount) then immediately call `componentWillUnmount`, and then call `componentDidMount` again. This helps you notice if you forgot to implement `componentWillUnmount` or if its logic doesn’t fully “mirror” what `componentDidMount` does.

### `render()`

The `render` method is the only required method in a class component.

The `render` method should specify what you want to appear on the screen, for example:

```jsx
import { Component } from 'react';  

class Greeting extends Component {  
	render() {  
		return <h1>Hello, {this.props.name}!</h1>;  
	}  
}
```

React may call `render` at any moment, so you shouldn’t assume that it runs at a particular time. Usually, the `render` method should return a piece of [JSX](https://react.dev/learn/writing-markup-with-jsx), but a few [other return types](https://react.dev/reference/react/Component#render-returns) (like strings) are supported. To calculate the returned JSX, the `render` method can read [`this.props`](https://react.dev/reference/react/Component#props), [`this.state`](https://react.dev/reference/react/Component#state), and [`this.context`](https://react.dev/reference/react/Component#context).

You should write the `render` method as a pure function, meaning that it should return the same result if props, state, and context are the same. It also shouldn’t contain side effects (like setting up subscriptions) or interact with the browser APIs. Side effects should happen either in event handlers or methods like [`componentDidMount`.](https://react.dev/reference/react/Component#componentdidmount)

#### Parameters 

`render` does not take any parameters.
#### Returns 

`render` can return any valid React node. This includes React elements such as `<div />`, strings, numbers, [portals](https://react.dev/reference/react-dom/createPortal), empty nodes (`null`, `undefined`, `true`, and `false`), and arrays of React nodes.

#### Caveats 

- `render` should be written as a pure function of props, state, and context. It should not have side effects.

- `render` will not get called if [`shouldComponentUpdate`](https://react.dev/reference/react/Component#shouldcomponentupdate) is defined and returns `false`.

- When [Strict Mode](https://react.dev/reference/react/StrictMode) is on, React will call `render` twice in development and then throw away one of the results. This helps you notice the accidental side effects that need to be moved out of the `render` method.

- There is no one-to-one correspondence between the `render` call and the subsequent `componentDidMount` or `componentDidUpdate` call. Some of the `render` call results may be discarded by React when it’s beneficial.

### `setState(nextState, callback?)`

Call `setState` to update the state of your React component.

```jsx
class Form extends Component {  

state = {  
	name: 'Taylor',  
};  

handleNameChange = (e) => {  
	const newName = e.target.value;  
	this.setState({  
		name: newName  
	});  
}  
 
render() {  
	return (  
		<>  
			<input value={this.state.name} onChange={this.handleNameChange} />  
			<p>Hello, {this.state.name}.</p>  
		</>  
	);  
}  
}
```

`setState` enqueues changes to the component state. It tells React that this component and its children need to re-render with the new state. This is the main way you’ll update the user interface in response to interactions.

Calling `setState` **does not** change the current state in the already executing code:

```jsx
function handleClick() {  
	console.log(this.state.name); // "Taylor"  
	this.setState({  
		name: 'Robin'  
	});  
	console.log(this.state.name); // Still "Taylor"!  
}
```

It only affects what `this.state` will return starting from the _next_ render.

You can also pass a function to `setState`. It lets you update state based on the previous state:

```jsx
handleIncreaseAge = () => {  
	this.setState(prevState => {  
	return {  
		age: prevState.age + 1  
		};  
	});  
}
```

You don’t have to do this, but it’s handy if you want to update state multiple times during the same event.

#### Caveats 

- Think of `setState` as a _request_ rather than an immediate command to update the component. When multiple components update their state in response to an event, React will batch their updates and re-render them together in a single pass at the end of the event. In the rare case that you need to force a particular state update to be applied synchronously, you may wrap it in [`flushSync`,](https://react.dev/reference/react-dom/flushSync) but this may hurt performance.

- `setState` does not update `this.state` immediately. This makes reading `this.state` right after calling `setState` a potential pitfall. Instead, use [`componentDidUpdate`](https://react.dev/reference/react/Component#componentdidupdate) or the setState `callback` argument, either of which are guaranteed to fire after the update has been applied. If you need to set the state based on the previous state, you can pass a function to `nextState` as described above.

Calling `setState` in class components is similar to calling a [`set` function](https://react.dev/reference/react/useState#setstate) in function components.

### `shouldComponentUpdate(nextProps, nextState, nextContext)`

If you define `shouldComponentUpdate`, React will call it to determine whether a re-render can be skipped.

If you are confident you want to write it by hand, you may compare `this.props` with `nextProps` and `this.state` with `nextState` and return `false` to tell React the update can be skipped.

```jsx
class Rectangle extends Component {  

	state = {  
		isHovered: false  
	};  

  

shouldComponentUpdate(nextProps, nextState) {  

if (nextProps.position.x === this.props.position.x &&  
	nextProps.position.y === this.props.position.y &&  
	nextProps.size.width === this.props.size.width &&  
	nextProps.size.height === this.props.size.height &&  
	nextState.isHovered === this.state.isHovered ) {  
	// Nothing has changed, so a re-render is unnecessary  
	return false;  
	}  
	return true;  
}  
// ...  
}
```

React calls `shouldComponentUpdate` before rendering when new props or state are being received. Defaults to `true`. This method is not called for the initial render or when [`forceUpdate`](https://react.dev/reference/react/Component#forceupdate) is used.

Return `true` if you want the component to re-render. That’s the default behavior.

Return `false` to tell React that re-rendering can be skipped.

#### Caveats 

- This method _only_ exists as a performance optimization. If your component breaks without it, fix that first.

- Consider using [`PureComponent`](https://react.dev/reference/react/PureComponent) instead of writing `shouldComponentUpdate` by hand. `PureComponent` shallowly compares props and state, and reduces the chance that you’ll skip a necessary update.

- We do not recommend doing deep equality checks or using `JSON.stringify` in `shouldComponentUpdate`. It makes performance unpredictable and dependent on the data structure of every prop and state. In the best case, you risk introducing multi-second stalls to your application, and in the worst case you risk crashing it.

- Returning `false` does not prevent child components from re-rendering when _their_ state changes.

- Returning `false` does not _guarantee_ that the component will not re-render. React will use the return value as a hint but it may still choose to re-render your component if it makes sense to do for other reasons.

### Note:

Optimizing class components with `shouldComponentUpdate` is similar to optimizing function components with [`memo`.](https://react.dev/reference/react/memo) Function components also offer more granular optimization with [`useMemo`.](https://react.dev/reference/react/useMemo)

### `static getDerivedStateFromProps(props, state)`

If you define `static getDerivedStateFromProps`, React will call it right before calling [`render`,](https://react.dev/reference/react/Component#render) both on the initial mount and on subsequent updates. It should return an object to update the state, or `null` to update nothing.

This method exists for [rare use cases](https://legacy.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#when-to-use-derived-state) where the state depends on changes in props over time. For example, this `Form` component resets the `email` state when the `userID` prop changes:

```jsx
class Form extends Component {  

state = {  
	email: this.props.defaultEmail,  
	prevUserID: this.props.userID  
};  

static getDerivedStateFromProps(props, state) {  

// Any time the current user changes,  
// Reset any parts of state that are tied to that user.  
// In this simple example, that's just the email.  
if (props.userID !== state.prevUserID) {  
	return {  
		prevUserID: props.userID,  
		email: props.defaultEmail  
	};  
}  
return null;  
}  
// ...  
}
```

Note that this pattern requires you to keep a previous value of the prop (like `userID`) in state (like `prevUserID`).

### Note:

Implementing `static getDerivedStateFromProps` in a class component is equivalent to [calling the `set` function from `useState` during rendering](https://react.dev/reference/react/useState#storing-information-from-previous-renders) in a function component.


### Define a class component

```jsx
import { Component } from 'react';

class Greeting extends Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

export default function App() {
  return (
    <>
      <Greeting name="Sara" />
      <Greeting name="Cahal" />
      <Greeting name="Edite" />
    </>
  );
}
```

Note that Hooks (functions starting with `use`, like [`useState`](https://react.dev/reference/react/useState)) are not supported inside class components.

### Migrating a component with state from a class to a function.

Class Component

```jsx
import { Component } from 'react';

export default class Counter extends Component {
  state = {
    name: 'Taylor',
    age: 42,
  };

  handleNameChange = (e) => {
    this.setState({
      name: e.target.value
    });
  }

  handleAgeChange = (e) => {
    this.setState({
      age: this.state.age + 1 
    });
  };

  render() {
    return (
      <>
        <input
          value={this.state.name}
          onChange={this.handleNameChange}
        />
        <button onClick={this.handleAgeChange}>
          Increment age
        </button>
        <p>Hello, {this.state.name}. You are {this.state.age}.</p>
      </>
    );
  }
}
```

Functional Component

```jsx
import { useState } from 'react';

export default function Counter() {
  const [name, setName] = useState('Taylor');
  const [age, setAge] = useState(42);

  function handleNameChange(e) {
    setName(e.target.value);
  }

  function handleAgeChange() {
    setAge(age + 1);
  }

  return (
    <>
      <input
        value={name}
        onChange={handleNameChange}
      />
      <button onClick={handleAgeChange}>
        Increment age
      </button>
      <p>Hello, {name}. You are {age}.</p>
    </>
  )
}
```

### Migrating a component with lifecycle methods from a class to a function 

Class Component

```jsx
import { Component } from 'react';
import { createConnection } from './chat.js';

export default class ChatRoom extends Component {
  state = {
    serverUrl: 'https://localhost:1234'
  };

  componentDidMount() {
    this.setupConnection();
  }

  componentDidUpdate(prevProps, prevState) {
    if (
      this.props.roomId !== prevProps.roomId ||
      this.state.serverUrl !== prevState.serverUrl
    ) {
      this.destroyConnection();
      this.setupConnection();
    }
  }

  componentWillUnmount() {
    this.destroyConnection();
  }

  setupConnection() {
    this.connection = createConnection(
      this.state.serverUrl,
      this.props.roomId
    );
    this.connection.connect();    
  }

  destroyConnection() {
    this.connection.disconnect();
    this.connection = null;
  }

  render() {
    return (
      <>
        <label>
          Server URL:{' '}
          <input
            value={this.state.serverUrl}
            onChange={e => {
              this.setState({
                serverUrl: e.target.value
              });
            }}
          />
        </label>
        <h1>Welcome to the {this.props.roomId} room!</h1>
      </>
    );
  }
}
```

Functional Component

```jsx
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

export default function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect(); // componentDidMount
    return () => {
      connection.disconnect(); // componentWillUnmount
    };
  }, [roomId, serverUrl]); // componentDidUpdate - if the dependencies are changed

  return (
    <>
      <label>
        Server URL:{' '}
        <input
          value={serverUrl}
          onChange={e => setServerUrl(e.target.value)}
        />
      </label>
      <h1>Welcome to the {roomId} room!</h1>
    </>
  );
}
```

### Migrating a component with context from a class to a function

In this example, the `Panel` and `Button` class components read [context](https://react.dev/learn/passing-data-deeply-with-context) from [`this.context`:](https://react.dev/reference/react/Component#context)
```jsx
import { createContext, Component } from 'react';

const ThemeContext = createContext(null);

class Panel extends Component {
  static contextType = ThemeContext;

  render() {
    const theme = this.context;
    const className = 'panel-' + theme;
    return (
      <section className={className}>
        <h1>{this.props.title}</h1>
        {this.props.children}
      </section>
    );    
  }
}

class Button extends Component {
  static contextType = ThemeContext;

  render() {
    const theme = this.context;
    const className = 'button-' + theme;
    return (
      <button className={className}>
        {this.props.children}
      </button>
    );
  }
}

function Form() {
  return (
    <Panel title="Welcome">
      <Button>Sign up</Button>
      <Button>Log in</Button>
    </Panel>
  );
}

export default function MyApp() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  )
}
```

When you convert them to function components, replace `this.context` with [`useContext`](https://react.dev/reference/react/useContext) calls:

```jsx
import { createContext, useContext } from 'react';

const ThemeContext = createContext(null);

function Panel({ title, children }) {
  const theme = useContext(ThemeContext);
  const className = 'panel-' + theme;
  return (
    <section className={className}>
      <h1>{title}</h1>
      {children}
    </section>
  )
}

function Button({ children }) {
  const theme = useContext(ThemeContext);
  const className = 'button-' + theme;
  return (
    <button className={className}>
      {children}
    </button>
  );
}

function Form() {
  return (
    <Panel title="Welcome">
      <Button>Sign up</Button>
      <Button>Log in</Button>
    </Panel>
  );
}

export default function MyApp() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  )
}
```

