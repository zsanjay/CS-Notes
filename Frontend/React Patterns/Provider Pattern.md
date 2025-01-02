
Make data available to multiple child components.

The Provider Pattern uses React's **Context** API - which is a way to easily share data between components.

Let's say that we want to add a theme toggle to our landing page, where users can switch between light mode and dark mode.

![[20250101123203.png]]

Several components change their style based on the currently active theme, such as the TopNav, the **Listing** cards, the **Main** section, and the **Toggle**.

With the Provider pattern, we can share the theme state across multiple components throughout our application. The provider provides this context to components, which in turn consume this data.

![[20250101123627.png]]

### Prop drilling

Before the Context API was available, we often ended up with something called prop drilling  when we wanted to share data across multiple components. This is the case when we pass props far down the component tree.

![[20250101123854.png]]

However, passing props down this way can get quite insecure and complex, and it's simply not a very scalable approach. We cannot easily rename a **prop**, or restructure the component tree. It can also easily lead to a decreased performance, since all child components need to re-render on a state update, even if they aren't consuming that data.

The provider pattern solves this by exposing the values of the **Context** to all children within a **Provider**. A component can optionally consume this data, making it possible to pass data to multiple components without prop drilling.


![[20250101125123.png]]

Only the components that care about the data get re-rendered when the state updates.

#### Implementation

A **Provider** is a higher order component provided to us by the **Context** object. We can create a **Context** object, using the **createContext** method that React provides for us.

```jsx
export const ThemeContext = React.createContext(null);

export function ThemeProvider({ children }) {
	const [ theme, setTheme ] = React.useState("light");

	return (
		<ThemeContext.Provider value = {{ theme, setTheme }}>
			{children}
		</ThemeContext.Provider>	
	);
}
```

Any component wrapped in the **ThemeProvider** now has access to the **theme** and **setTheme** properties.

```jsx
import { ThemeProvider, ThemeContext } from '../context';

const LandingPage = () => {
	<ThemeProvider>
		<TopNav />
		<Toggle />
	</ThemeProvider>;
};

const TopNav = () => {
	return (
		<ThemeContext.Consumer>
			{{ theme }} => {
			<div style={{ backgroundColor : theme === "light" ? "#fff" : "#000"               }}>
			...
			</div>
			}
		</ThemeContext.Consumer>
	);
};

const Toggle = () => {
	return (
		<ThemeContext.Consumer>
			{{ theme, setTheme }} => (
			<button
			  onClick={() => setTheme(theme === "light" ? "dark" : "light")}                   style={{ 
				backgroundColor: theme === "light" ? "#fff" : "#000",
				color: theme === "light" ? "#000" : "#fff",	
			}}>
				Use {theme === "light" ? "Dark" : "Light"} Theme
			</button>	
			)}
		</ThemeContext.Consumer>
	);
};
```

However, we can also combine the Provider with the Hooks pattern. Instead of wrapping components in a **<ThemeContext.Consumer>** component, we can use the built-in **useContext** hook.

```jsx
export const ThemeContext = React.createContext(null);

export function useThemeContext() {
	const { theme, setTheme } = useContext(ThemeContext);
	return { theme, setTheme };
}

export function ThemeProvider({ children }) {
	const [theme, setTheme] = React.useState("light");

	return (
		<ThemeContext.Provider value={{ theme, setTheme }}>
			{children}
		</ThemeContext.Provider>
	)
}
```

Each component that needs to have access to the **ThemeContext**, can now simply use the **useThemeContext** hook.

```jsx
import { useThemeContext } from '../context';

const LandingPage = () => {
	<ThemeProvider>
		<TopNav />
		<Toggle />
	</ThemeProvider>;	
}

const TopNav = () => {
	const { theme } = useThemeContext();
	return (
	    <div style={{ backgroundColor: theme === "light" ? "#fff" : "#000 " }}>
	      ...
	    </div>
	);
};

const Toggle = () => {
	const { theme, setTheme } = useThemeContext();
	return (
		<button
			onClick={() => setTheme(theme === "light" ? "dark" : "light")}
			style={{
				backgroundColor: theme === "light" ? "#fff" : "#000",
				color : theme === "light" ? "#000" : "#fff"
			}}>
			Use { theme === "light" ? "Dark" : "Light"} Theme
		</button>	
	);
};
```

By creating hooks for the different contexts, it's easy to separate the providers's logic from the components that render the data.

### Tradeoffs

#### Pros:

**Scalability** : There's less risk involved when sharing state across multiple components with the Provider Pattern, as we can easily rename values when our application grows, and easily reuse components.

#### Cons:

**Performance** : Components that consume the **Provider's** context re-render whenever a value changes. This can cause performance issues, if you aren't careful which components are consuming the context.

### Exercise

#### Challenge

The application below contains a **Listings** component and an **Input** component that both use the **useListings** hook. However, this results in two separate calls to the API. Refactor this code so that it uses a **ListingsProvider** that provides the listings data to both components.


#### useListings hook

```tsx
import React from 'react';

export default function useListings() {
	const [listings, setListings] = React.useState(null);
	React.useEffect(() => {
		fetch('https://house-lydiahallie.vercel.app/api/listings')
		.then((res) => res.json())
		.then((res) => setListings(res.listings));
	}, []);
	return listings;
}
```

#### ListingsProvider

```tsx
import React, { useContext } from 'react';
import useListings from '../hooks/useListings';

export const ListingsContext = React.createContext(null);

export function useListingsContext() {
	return React.useContext(ListingsContext);
}

export function ListingsProvider({ children }) {
	const listings = useListings();
	if (!listings) return null;
	
	return (
		<ListingsContext.Provider value={listings}>
			{children}
		</ListingsContext.Provider>
	);
}
```

#### App.tsx

```tsx
import * as React from 'react';
import './style.css';
import Listings from './components/Listings';
import Input from './components/Input';
import { ListingsProvider } from './context/ListingsProvider';

export default function App() {
	return (
		<ListingsProvider>
			<div
				style={{
					display: 'flex',
					flexDirection: 'column',
					justifyContent: 'center',
					alignItems: 'center',
					padding: '3em',
				}}>					
				<Input />
				<Listings />
			</div>
		</ListingsProvider>
	);
}
```

#### Input.tsx

```tsx
import React from 'react';
import { useListingsContext } from '../context/ListingsProvider';

export default function Input(props) {
	const listings = useListingsContext();
	const [open, setOpen] = React.useState(false);
	const [value, setValue] = React.useState('');
	const toggle = React.useCallback(() => setOpen((state) => !state), []);

	return (
		<div className="flyout">
			<input
				onFocus={toggle}
				onBlur={toggle}
				onChange={(e) => setValue(e.target.value)}
				className="flyout-input"
				value={value}
				placeholder="Enter an address, city ,or ZIP code"
				{...props}/>
		
		{open && (
		<div className="flyout-list">
			<ul>
				{listings.map((listing) => (
					<li
						key={listing.id}
						onMouseDown={() => {setValue(listing.name)}}
						className="flyout-list-item">					
						{listing.name}
					</li>
				))}
			</ul>
		</div>
		)}
		</div>
	);
}
```

#### Listings

```tsx
import React from 'react';
import { Listing } from './Listing';
import { ListingsGrid } from './ListingsGrid';
import { useListingsContext } from '../context/ListingsProvider';

export default function Listings() {
	const listings = useListingsContext();
	if (!listings) return null;
  
	return (
		<ListingsGrid>
			{listings.map((listing) => (
			<Listing key={listing.id} listing={listing} />))}
		</ListingsGrid>
	);
}
```


