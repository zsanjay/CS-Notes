
Pass JSX elements to components through props

With the Render Props pattern, we pass components as props to other components. The components that are passed as props can in turn receive props from that component.

Render props make it easy to reuse logic across multiple components.

![[20250101100552.png]]

#### Implementation

If we wanted to implement an input field with which a user can convert a temperature from Celsius to Kelvin and Fahrenheit, we can use the **renderKelvin** and **renderFahrenheit** render props.

These props receive the **value** of the input, which they convert to the correct temperature in either K or °F.

```jsx
function Input(props) {
	const [value, setValue] = useState("");

	return (
		<>
			<input value={value} onChange={{e} => setValue(e.target.value)} />
			{props.renderKelvin({ value: value + 273.15 })}
			{props.renderFahrenheit({value : (value * 9) / 5 + 32 })}
		</>	
	);
}

export default function App() {
	return (
		<Input 
			renderKelvin={({ value }) => <div className="temp">{value}K</div>}
			renderFahrenheit={({ value }) => <div className="temp">{value}°F</div>}
		/>	
	);
}
```


### Tradeoffs

#### Pros:

**Reusability** : Since render props can be different each time, we can make components that receive render props highly reusable for multiple use cases.

**Separation of concerns :** We can separate our app's logic from rendering components through render props. The stateful component that receives a render prop can pass the data onto stateless components, which merely render the data.

**Solution to HOC problems** : Since we explicitly pass props, we solve the HOC's implicit props issue. The props that should get passed down to the element, are all visible in the render prop's arguments list. We know exactly where certain props come from.

#### Cons:

**Unnecessary with Hooks** : Hooks changed the way we can add reusability and data sharing to components, which can replace the render props pattern in many cases.

### Exercise

#### Challenge

Refactor the following code so that **TemperatureConverter** uses the **renderKelvin** and **renderFahrenheit** props to render the **<Kelvin />** and **<Fahrenheit />** components.

#### App.tsx

```tsx
import * as React from 'react';
import './style.css';
import TemperatureConverter, {Kelvin, Fahrenheit} from './TemperatureConverter';

export default function App() {
	return (
		<TemperatureConverter
			renderKelvin={({ value }) => <Kelvin value={value} />}
			renderFahrenheit={({ value }) => <Fahrenheit value={value} />}
		/>
	);
}
```

#### TemperatureConverter.tsx

```tsx
import React from 'react';

export function Kelvin({ value }) {
	return (
		<div className="temp-card">
		The temperature in Kelvin is: <span className="temp">{value}K</span>
		</div>
	);
}

export function Fahrenheit({ value }) {
	return (
		<div className="temp-card">
			The temperature in Fahrenheit is:
			<span className="temp">{value}°F</span>
		</div>
	);
}

export default function TemperatureConverter(props) {
	const [value, setValue] = React.useState(0);
	return (
		<div className="card">
			<input
			type="number"
			placeholder="Degrees Celcius"
			onChange={(e) => setValue(parseInt(e.target.value))}
			/>
				{props.renderKelvin({ value: value + 273.15 })}
				{props.renderFahrenheit({ value: (value * 9) / 5 + 32 })}
		</div>
	);
}
```

