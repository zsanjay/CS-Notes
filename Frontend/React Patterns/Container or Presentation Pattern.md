
Enforce separation of concerns by separating the view from the application logic.

We can use the Container/Presentational pattern to separate the logic of a component from the view. To achieve this, we need to have a:

- **Presentational** Component, that cares about how data is shown to the user.
- **Container** Component, that cares about what data is shown to the user.

For example, if we wanted to show listings on the landing page, we could use a container component to fetch the data for the recent listings, and use a presentational component to actually render this data.

![[20241231204148.png]]

![[20241231204215.png]]

#### Implementation

We can implement the Container/Presentational pattern using either Class Components or functional components.

```jsx
import React from 'react';
import { LoadingListings, Listings, ListingsGrid } from '../components';

function ListingsContainerComponent() {
	const [listings, setListings] = React.useState([]);

	React.useEffect(() => {
		fetch("https://my.cms.com/listings")
		.then((res) => res.json())
		.then((res) => setListings(res.listings));
	}, []); 

	return <Listings listings={listings} />;
}

function ListingsPresentationalComponent(props) {
	if(props.listings.length === 0) {
		return <LoadingListings />;
	}

	return (
		<ListingsGrid>
			{listings.map((listing) => (
				<Listing listing={listing} />
			))}
		</ListingsGrid>
	);
}
 ```

### Tradeoffs

#### Pros

**Separation of concerns**: Presentational components can be pure functions which are responsible for the UI, whereas container components are responsible for the state and data of the application. This makes it easy to enforce the separation of concerns.

**Reusability**: We can easily reuse the presentational components throughout our application for different purposes.

**Flexibility**: Since presentational components don't alter the application logic, the appearance of presentational components can easily be altered by someone without knowledge of the codebase, for example a designer. If the presentational component was reused in many parts of the application, the change can be consistent throughout the app.

**Testing**: Testing presentational components is easy, as they are usually pure functions. We know what the components will render based on which data we pass, without having to mock a data store.

#### Cons

**Not necessary with Hooks**: Hooks make it possible to achieve the same result without having to use the Container/Presentational pattern.

For example, we can use SWR to easily fetch data and conditionally render the listings or the skeleton component. Since we can use hooks in functional components and keep the code small and maintainable, we don't have to split the component into a container and presentational component.

#### SWR

https://swr.vercel.app/

The name “SWR” is derived from `stale-while-revalidate`, a HTTP cache invalidation strategy popularized by [HTTP RFC 5861(opens in a new tab)](https://tools.ietf.org/html/rfc5861). SWR is a strategy to first return the data from cache (stale), then send the fetch request (revalidate), and finally come with the up-to-date data.

```jsx
import React from 'react';
import useSWR from 'swr';
import { LoadingListings, Listing, ListingsGrid } from '../components';

function Listings(props) {
	const {
		data: listings,
		loading,
		error,
	} = useSWR("https://my.cms.com/listings", (url) => 
		fetch(url).then((r) => r.json())
	);

	if(loading) {
		return <LoadingListings />;
	}

	return (
		<ListingsGrid>
			{listings.map((listing) => (
				<Listing listing={listing} />
			))}
		</ListingGrid>	
	);
}
```

#### Overkill

The Container/Presentational pattern can easily be an overkill in smaller sized application.

### Exercise

We have a Listings component that both contains the fetching logic, as well as the rendering logic. In order to make the fetching logic more reusable, we can split this component up into a container and presentational component.

#### Challenge

Remove the data fetching logic from the Listings component, and move this to a separate ListingsContainer component located in /components/container/Listings.tsx

#### Listings.tsx - Container

```tsx
import React from 'react';

import { Listings } from '../presentational/Listings';

export default function ListingsContainerComponent() {

	const [data, setData] = React.useState(null);

	React.useEffect(() => {
		fetch('https://house-lydiahallie.vercel.app/api/listings')
		.then((res) => res.json())
		.then((res) => setData(res));
	}, []);

	if (!data) return null;
	return <Listings listings={data.listings} />;

}
```

#### Listings.tsx - Presentation

```tsx
import React from 'react';
import { Listing } from './Listing';
import { ListingsGrid } from './ListingsGrid';

export function Listings(props) {
	return (
		<ListingsGrid>
		{props.listings.map((listing) => (
			<Listing key={listing.id} listing={listing} />
		))}
		</ListingsGrid>
	);
}
```

#### References

https://javascriptpatterns.vercel.app/patterns/react-patterns/conpres