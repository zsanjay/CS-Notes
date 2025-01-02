Use functions to reuse stateful logic among multiple components throughout the app.

React Hooks are functions special types of functions that you can use in order to:

- Add state to a functional component.
- Reuse stateful logic among multiple components throughout the app.
- Manage a component's lifecycle.

Besides built-in hooks, such as **useState**, **useEffect**, and **useReducer**, we can create custom hooks to easily share stateful logic across multiple components within the application.

For example, if we wanted to see if a certain component is currently being hovered, we can create and use a **useHover** hook.

![[20250101110015.png]]

We could just add this logic to the **Listing** component itself. However, in order to make the hover logic reusable  throughout the application, it makes more sense to create its own hook for it.

![[20250101110438.png]]

Now that it's a hook, we can reuse the same logic throughout multiple components in our application. For example, if we also wanted to add the hover logic to the **Image** and **Button** component, we can simply use the hook within these functional components.

![[20250101110712.png]]
#### Implementation

React knows a hook is a hook when the name starts with **use**. Since this hook keeps track of the hovering state, it makes sense to name it **useHover**.

Within this hook, we have to keep track of the hovering state by using three build-in hooks:

1. **useState**, which keeps track of whether the component is currently being hovered.
2. **useRef**, which creates a ref to the component that we're tracking.
3. **useEffect**, which gets executed as soon as the **ref** has a value. In here, we can add event listeners to the component to keep track of the hovering state.

```jsx
export function useHover() {
	const [isHovering, setIsHovering] = React.useState(false);
	const ref = React.useRef(null)'

	const handleMouseOver = () => setIsHovering(true);
	const handleMouseOut = () => setIsHovering(false);

	React.useEffect(() => {
		const node = ref.current;
		if(node) {
			node.addEventListener("mouseover", handleMouseOver);
			node.addEventListener("mouseout", handleMouseOut);
			return () => {
				node.removeEventListener("mouseover", handleMouseOver);
				node.removeEventListener("mouseout", handleMouseOut);
			};
		}
	}, [ref.current]);

	return [ref, isHovering];
}
```

We can use this hook in any component that cares about the hovering state.

```jsx
import { useHover } from "../hooks/useHover";

export function Listing() {
	const [ref, isHovering] = useHover();

	React.useEffect(() => {
		if(isHovering) {
			// Add logic here
		}
	}, [isHovering]);

	return (
		<div ref={ref}>
			<ListingCard />
		</div>
	);
}
```

### Tradeoffs

#### Pros

**Simplifies components** : Hooks make it easy to add state to functional compontents, rather then (usually more complex) class components.

**Reusing stateful logic** : Hooks allow you to reuse stateful logic among multiple components across the application, which reduces the chances of errors,  and allows for composition with plain functions.

**Sharing non-visual logic** : Hooks make it easy to share non-visual logic, without having to use patterns like HOC or Render Props.

**Good alternative to older React design patterns** : The hooks pattern is a good alternative to an older React design pattern, which is mainly used with Class components, namely the Presentational / Container pattern.

![[20250101120144.png]]
With Hooks, we are no longer have to wrap Presentational components in Container components to pass data. Instead, we can directly use the Hook within the presentational component.

![[20250101120654.png]]
**Rules of Hooks**

Hooks require certain rules to be followed. without a linter plugin, it is difficult to know which rule has been broken, and you can accidentally end up using the wrong built-in hook.


### Exercise

#### Challenge

The code below is using the Container/ Presentational pattern to display the listings. Refactor this code so that it uses a hook instead of a container component.

#### listings.tsx

```tsx
import React from 'react';

export default function useListings() {
	const [listings, setListings] = React.useState([]);
	React.useEffect(() => {
		fetch('https://house-lydiahallie.vercel.app/api/listings')
		.then((res) => res.json())
		.then(({ listings }) => setListings(listings));
	}, []);
	
	if (!listings) return null;
	return listings;
}
```

#### Listings.tsx - Presentational Component

```tsx
import React from 'react';
import { Listing } from './Listing';
import { ListingsGrid } from './ListingsGrid';
import useListings from '../../hooks/listings';

export function Listings() {
	const listings = useListings();
	return (
		<ListingsGrid>
			{listings.map((listing) => (
		<Listing key={listing.id} listing={listing} />
	))}
	</ListingsGrid>
	);
}
```


