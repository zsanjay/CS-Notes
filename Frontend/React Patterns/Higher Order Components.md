
Pass reusable logic down as props to components throughout your application.

Higher-Order Components (HOC) make it easy to pass logic to components by wrapping them.

For example, if we wanted to easily change the styles to a text by making the font larger and the font weight bolder, we could create two Higher-Order Components:

- **withLargeFontSize**, which appends the **font-size: "90px"** field to the style attribute.
- **withBoldFontWeight**, which appends the **font-weight: "bold"** field to the style attribute.

![[20250101091656.png]]

Any component that's wrapped with either of these higher-order components will get a larger font size, a bolder font weight, or both!

### Implementation

We can apply logic to another component, by:

1. Receiving another component as its props.
2. Applying additional logic to the passed component.
3. Returning the same or a new component with additional logic.

![[20250101092100.png]]
To implement the above example, we can create a **withStyles** HOC that adds a **color** and **font-size** prop to the component's style.

```jsx
export function withStyles(Component) {
  return (props) => {
    const style = {
      color: "red",
      fontSize: "1em",
      // Merge props
      ...props.style,
    };

    return <Component {...props} style={style} />;
  };
}
```

We can import the **withStyles** HOC, and wrap any component that needs styling.

```jsx
import { withStyles } from "./hoc/withStyles";

const Text = () => <p style={{ fontFamily: "Inter" }}>Hello world!</p>;
const StyledText = withStyles(Text);
```

Note - If you have a component that always needs to be wrapped within a HOC, you can also directly pass it instead of creating two separate components like we did in the example above.

```jsx
const Text = withStyles(() => (
	<p style={{ fontFamily: "Inter" }}>Hello world!</p>
));
```

### Tradeoffs

#### Pros :

**Separation of concerns** : Using the Higher-Order-Component pattern allows us to keep logic that we want to re-use all in one place. This reduces the risk of accidentally spreading bugs throughout the application by duplicating code over and over, potentially introducing new bugs each time.

#### Cons :

**Naming collisions** : It can easily happen that the HOC overrides a prop of a component. Make sure that the HOC can handle accidental name collision, by either renaming the prop or merging the props.

```jsx
function withStyles(Component) {
  return props => {
    const style = {
      padding: '0.2rem',
      margin: '1rem',
      // Merge props
      ...props.style
    }

    return <Component {...props} style={style} />
  }
}

// The `Button` component has a `style` prop, that shouldn't get overwritten in the HOC.
const Button = () = <button style={{ color: 'red' }}>Click me!</button>
const StyledButton = withStyles(Button)
```

**Readability** : When using multiple composed HOCs that all pass props to the element that's wrapped within them, it can be difficult to figure out which HOC is responsible for which prop. This can hinder debugging and scaling an application easily.

### Exercise

If we have a lot of components that fetch data, we might want to show a certain loader while they're still loading data.

In that case, we might want to create a **withLoader** HOC, which returns a component that fetches data, and returns a **LoadingSpinner** component while it's fetching data.

##### withLoader.tsx

```jsx
import React from 'react';
import { LoadingSpinner } from '../components/LoadingSpinner';

export default function withLoader(Element, url) {
	return (props) => {
	const [data, setData] = React.useState(null);
	React.useEffect(() => {
		fetch(url)
		.then((res) => res.json())
		.then((res) => setData(res));
	}, []);
	
    if (!data) return <LoadingSpinner />;
	return <Element {...props} data={data} />;
};
}
```

##### Listings.tsx

```jsx
import React from 'react';
import withLoader from '../../hoc/withLoader';
import { Listing } from './Listing';
import { ListingsGrid } from './ListingsGrid';

export function Listings(props) {
	return (
		<ListingsGrid>
			{props.data.listings.map((listing) => (
				<Listing key={listing.id} listing={listing} />
			))}
		</ListingsGrid>
		);
	}

export default withLoader(
	Listings,
	'https://house-lydiahallie.vercel.app/api/listings'
);
```

