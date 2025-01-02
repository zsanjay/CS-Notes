Create multiple components that work together to perform a single task.

With the Compound Pattern, we can create multiple components that work together to perform one single task.

Let's say for example that we have a **Search** input component. When a user clicks on the search input, we show a **SearchPopup** component that shows some popular locations.

![[20250101153838.png]]

To create this behavior, we can create a FlyOut compound component.

![[20250101154032.png]]

This **FlyOut** component is an example of a compound component, as it also exposes some sub-components that all work together to toggle and render the **FlyOut** component.

```jsx
import React from "react";
import { FlyOut } from "./FlyOut";

export default function SearchInput() {
	return (
	<FlyOut>
		<FlyOut.Input placeholder="Enter an address, city, or ZIP code" />
		<FlyOut.List>
			<FlyOut.ListItem value="San Francisco, CA">
				San Francisco, CA
			<FlyOut.ListItem>	
	        <FlyOut.ListItem value="Seattle, WA">Seattle, WA</FlyOut.ListItem>
	        <FlyOut.ListItem value="Austin, TX">Austin, TX</FlyOut.ListItem>
	        <FlyOut.ListItem value="Miami, FL">Miami, FL</FlyOut.ListItem>
	        <FlyOut.ListItem value="Boulder, CO">Boulder, CO</FlyOut.ListItem>
	    <FlyOut.List>
	</FlyOut>        
	);
}
```

The **FlyOut** compound component is a stateful component - which means we don't have to add the stateful logic to the **SearchInput** component.

### Implementation

We can implement the Compound pattern using either a Provider, or React.Children.map.

#### Provider

The **FlyOut** compound component consists of:

- **FlyoutContext** to keep track of the visibility state of FlyOut.
- **Input** to toggle the **FlyOut's List** component's visibility.
- **List** to render the **FlyOut's ListItems**.
- **ListItem** that gets rendered within the **List**.

```jsx
const FlyOutContext = React.createContext();

export function FlyOut(props) {
	const [open, setOpen] = React.useState(false);
	const [value, setValue] = React.useState("");
	const toggle = React.useCallback(() => setOpen((state) => !state), []);

	return (
		<FlyOutContext.Provider value={{ open, toggle, value, setValue }}>
			<div>{props.children}</div>
		</FlyOutContext.Provider>	
	);
}

function Input(props) {
	const { value, toggle } = React.useContext(FlyOutContext);
	return <input onFocus={toggle} onBlur={toggle} value={value} {...props} />;
}

function List({ children }) {
	const { open } = React.useContext(FlyOutContext);
	return open && <ul>{children}</ul>;
}

function ListItem({ children, value }) {
	const { setValue } = React.useContext(FlyOutContext);
	return <li onMouseDown={() => setValue(value)}>{children}</li>;
}

FlyOut.Input = Input;
FlyOut.List = List;
FlyOut.ListItem = ListItem;
```

Although we didn't have to name our compound component's sub-components 
```jsx
FlyOut<ComponentName>
```
, it's an easy way to identify compound components, and only requires a single import.


#### React.Children.map

Another way to implement the Compound pattern, is to use **React.Children.map** in combination with **React.cloneElement**. Instead of having to use the Context API like in the previous example, we now have access to these two values through props.

```jsx
export function FlyOut(props) {
  const [open, setOpen] = React.useState(false);
  const [value, setValue] = React.useState("");
  const toggle = React.useCallback(() => setOpen((state) => !state), []);

  return (
    <div>
      {React.Children.map(props.children, (child) =>
        React.cloneElement(child, { open, toggle, value, setValue })
      )}
    </div>
  );
}

function Input(props) {
  const { value, toggle } = React.useContext(FlyOutContext);

  return <input onFocus={toggle} onBlur={toggle} value={value} {...props} />;
}

function List({ children }) {
  const { open } = React.useContext(FlyOutContext);

  return open && <ul>{children}</ul>;
}

function ListItem({ children, value }) {
  const { setValue } = React.useContext(FlyOutContext);

  return <li onMouseDown={() => setValue(value)}>{children}</li>;
}

FlyOut.Input = Input;
FlyOut.List = List;
FlyOut.ListItem = ListItem;
```

All children components are cloned, and passed the value of open, toggle, value and setValue.

### Tradeoffs

#### Pros:

**State management** : Compound components manage their own internal state, which they share among the several child components. When implementing a compound component, we don't have to worry about managing the state ourselves.

**Single Import** : When importing a compound component, we don't have to explicitly import the child components that are available on that component.

#### Cons:

**Nested components** : When using **React.Children.map**, only direct children of the parent component will have access to the open and toggle props, meaning we can't wrap any of these components in another component.

```jsx
function FlyoutMenu() {
  return (
    <FlyOut>
      {/* This breaks, since the direct child of FlyOut is now a div */}
      <div>
        <FlyOut.Input />
        <FlyOut.List>
          <FlyOut.ListItem>San Francisco, CA</FlyOut.ListItem>
          <FlyOut.ListItem>Seattle, WA</FlyOut.ListItem>
        </FlyOut.List>
      </div>
    </FlyOut>
  );
}
```

**Naming collisions** : Cloning an element with **React.cloneElement** performs a shallow merge. If we pass a prop that already exists on the component, in this example **open** or **toggle**, a naming collision occurs, and the value of these props will be overwritten withe the latest value that we pass.


### Challenge

#### Exercise

Create a `FlyOut` component that gets rendered by the `Input` component. This component should contain:

- `FlyOut.Input` to render the input field
- `FlyOut.List` to render the list items
- `FlyOut.ListItem` to render the list items


#### FlyOut Component

```tsx
import React from 'react';
const FlyOutContext = React.createContext();
  
function FlyOut(props) {
	const [open, setOpen] = React.useState(false);
	const [value, setValue] = React.useState('');
	const toggle = React.useCallback(() => setOpen((state) => !state), []);

	return (
		<FlyOutContext.Provider value={{ open, toggle, value, setValue }}>
			<div className="flyout">{props.children}</div>
		</FlyOutContext.Provider>
	);
}

function Input(props) {
	const { value, toggle, setValue } = React.useContext(FlyOutContext);
  
	return (
		<input
		onFocus={toggle}
		onBlur={toggle}
		className="flyout-input"
		value={value}
		onChange={(e) => setValue(e.target.value)}
		{...props}
		/>
	);
}

function List({ children }) {
	const { open } = React.useContext(FlyOutContext);
	return (
		open && (
			<div className="flyout-list">
				<ul>{children}</ul>
			</div>
		)
	);
}

function ListItem({ children, value }) {
	const { setValue } = React.useContext(FlyOutContext);
	return (
		<li
			onMouseDown={() => {
				setValue(value);
			}}
			className="flyout-list-item">			
			{children}
		</li>
	);
}

FlyOut.Input = Input;
FlyOut.List = List;
FlyOut.ListItem = ListItem;
  
export { FlyOut };
```

#### Input.tsx

```tsx
import React from 'react';
import { useListingsContext } from '../context/ListingsProvider';
import { FlyOut } from './FlyOut';

export default function Input(props) {
	const listings = useListingsContext();
	return (
		<FlyOut>
			<FlyOut.Input />
			<FlyOut.List>
				{listings.map((listing) => (
				<FlyOut.ListItem value={listing.name}>
					{listing.name}     
				</FlyOut.ListItem>
				))}
			</FlyOut.List>
		</FlyOut>
	);
}
```

