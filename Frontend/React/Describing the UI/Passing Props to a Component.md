React components use _props_ to communicate with each other. Every parent component can pass some information to its child components by giving them props. Props might remind you of HTML attributes, but you can pass any JavaScript value through them, including objects, arrays, and functions.

### Familiar props

Props are the information that you pass to a JSX tag. For example, `className`, `src`, `alt`, `width`, and `height` are some of the props you can pass to an `<img>`:

```jsx
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}

export default function Profile() {
  return (
    <Avatar />
  );
}
```

The props you can pass to an `<img>` tag are predefined (ReactDOM conforms to [the HTML standard](https://www.w3.org/TR/html52/semantics-embedded-content.html#the-img-element)). But you can pass any props to _your own_ components, such as `<Avatar>`, to customize them. Here’s how!

### Step 1: Pass props to the child component.

First, pass some props to `Avatar`. For example, let’s pass two props: `person` (an object), and `size` (a number):

```jsx
export default function Profile() {  
return (  
	<Avatar  
		person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}  
		size={100}  
	/>  
);  
}
```

#### Note

If double curly braces after `person=` confuse you, recall [they’re merely an object](https://react.dev/learn/javascript-in-jsx-with-curly-braces#using-double-curlies-css-and-other-objects-in-jsx) inside the JSX curlies.

### Step 2: Read props inside the child component.

You can read these props by listing their names `person, size` separated by the commas inside `({` and `})` directly after `function Avatar`. This lets you use them inside the `Avatar` code, like you would with a variable.

```jsx
function Avatar({ person, size }) {  
// person and size are available here  
}
```

```jsx
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <div>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi', 
          imageId: 'YfeOqp2'
        }}
      />
      <Avatar
        size={80}
        person={{
          name: 'Aklilu Lemma', 
          imageId: 'OKS67lh'
        }}
      />
      <Avatar
        size={50}
        person={{ 
          name: 'Lin Lanying',
          imageId: '1bX5QH6'
        }}
      />
    </div>
  );
}
```

Props let you think about parent and child components independently. For example, you can change the `person` or the `size` props inside `Profile` without having to think about how `Avatar` uses them. Similarly, you can change how the `Avatar` uses these props, without looking at the `Profile`.

You can think of props like “knobs” that you can adjust. They serve the same role as arguments serve for functions—in fact, props _are_ the only argument to your component! React component functions accept a single argument, a `props` object:

```jsx
function Avatar(props) {  
	let person = props.person;  
	let size = props.size;  
	// ...  
}
```

Usually you don’t need the whole `props` object itself, so you destructure it into individual props.

**Don’t miss the pair of `{` and `}` curlies** inside of `(` and `)` when declaring props:

```jsx
function Avatar({ person, size }) {  
	// ...
}
```

This syntax is called [“destructuring”](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Unpacking_fields_from_objects_passed_as_a_function_parameter) and is equivalent to reading properties from a function parameter:

```jsx
function Avatar(props) {  
	let person = props.person;  
	let size = props.size;  
	// ...
}
```

### Specifying a default value for a prop.

```jsx
function Avatar({ person, size = 100 }) {  
	// ...  
}
```

Now, if `<Avatar person={...} />` is rendered with no `size` prop, the `size` will be set to `100`.

The default value is only used if the `size` prop is missing or if you pass `size={undefined}`. But if you pass `size={null}` or `size={0}`, the default value will **not** be used.

### Forwarding props with the JSX spread syntax 

Sometimes, passing props gets very repetitive:

```jsx
function Profile({ person, size, isSepia, thickBorder }) {  
return (  
	<div className="card">  
	<Avatar  
		person={person}  
		size={size}  
		isSepia={isSepia}  
		thickBorder={thickBorder}  
	/>  
	</div>  
);  
}
```

There’s nothing wrong with repetitive code—it can be more legible. But at times you may value conciseness. Some components forward all of their props to their children, like how this `Profile` does with `Avatar`. Because they don’t use any of their props directly, it can make sense to use a more concise “spread” syntax:

```jsx
function Profile(props) {  
return (  
	<div className="card">  
		<Avatar {...props} />  
	</div>  
);  
}
```

**Use spread syntax with restraint.** If you’re using it in every other component, something is wrong. Often, it indicates that you should split your components and pass children as JSX.

### Passing JSX as children 

It is common to nest built-in browser tags:

```jsx
<div>  
	<img />  
</div>
```

Sometimes you’ll want to nest your own components the same way:

```jsx
<Card>  
	<Avatar />  
</Card>
```

When you nest content inside a JSX tag, the parent component will receive that content in a prop called `children`. For example, the `Card` component below will receive a `children` prop set to `<Avatar />` and render it in a wrapper div:

#### App.js

```jsx
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}
```

#### Avatar.js

```jsx
import { getImageUrl } from './utils.js';

export default function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}
```

### How props change over time.

The `Clock` component below receives two props from its parent component: `color` and `time`.

```jsx
export default function Clock({ color, time }) {
  return (
    <h1 style={{ color: color }}>
      {time}
    </h1>
  );
}
```

This example illustrates that **a component may receive different props over time.** Props are not always static! Here, the `time` prop changes every second, and the `color` prop changes when you select another color. Props reflect a component’s data at any point in time, rather than only in the beginning.

However, props are [immutable](https://en.wikipedia.org/wiki/Immutable_object)—a term from computer science meaning “unchangeable”. When a component needs to change its props (for example, in response to a user interaction or new data), it will have to “ask” its parent component to pass it _different props_—a new object! Its old props will then be cast aside, and eventually the JavaScript engine will reclaim the memory taken by them.

**Don’t try to “change props”.** When you need to respond to the user input (like changing the selected color), you will need to “set state”, which you can learn about in [State: A Component’s Memory.](https://react.dev/learn/state-a-components-memory)

### Challenge

#### Extract a component.

```jsx
import { getImageUrl } from './utils.js';

function Profile(props) {
  return (
    <section className="profile">
        <h2>{props.name}</h2>
        <img
          className="avatar"
          src={getImageUrl(props.imageId)}
          alt={props.name}
          width={70}
          height={70}
        />
        <ul>
          <li>
            <b>Profession: </b> 
            {props.profession}
          </li>
          <li>
            <b>Awards: {props.awardsCount} </b> 
            {props.awards}
          </li>
          <li>
            <b>Discovered: </b>
            {props.discovered}
          </li>
        </ul>
      </section>
  );
}

export default function Gallery() {
  return (
    <div>
      <h1>Notable Scientists</h1>
      <Profile 
        name="Maria Skłodowska-Curie" 
        imageId="szV5sdG" 
        profession="physicist and chemist" 
        awardsCount = "4"
        awards="(Nobel Prize in Physics, Nobel Prize in Chemistry, Davy Medal, Matteucci Medal)" 
        discovered="polonium (chemical element)" />
      <Profile 
        name="Katsuko Saruhashi" 
        imageId="YfeOqp2" 
        profession="geochemist" 
        awardsCount = "2"
        awards="(Miyake Prize for geochemistry, Tanaka Prize)" 
        discovered="a method for measuring carbon dioxide in seawater" />
     </div>
  );
}
```

#### Adjust the image size based on a prop

```jsx
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  const imageSize = size > 90 ? 's' : 'b';
  return (
    <img
      className="avatar"
      src={getImageUrl(person, imageSize)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <Avatar
      size={120}
      person={{ 
        name: 'Gregorio Y. Zara', 
        imageId: '7vQD0fP'
      }}
    />
  );
}
```

#### Passing JSX in a `children` prop

```jsx
function Card({ children }) {
  return (
    <div className="card">
      <div className="card-content">
        {children}
      </div>  
    </div>  
  );
}

export default function Profile() {
  return (
    <div>
      <Card>
        <h1>Photo</h1>
          <img
            className="avatar"
            src="https://i.imgur.com/OKS67lhm.jpg"
            alt="Aklilu Lemma"
            width={70}
            height={70}
          />
      </Card>
      <Card>
         <h1>About</h1>
          <p>Aklilu Lemma was a distinguished Ethiopian scientist who discovered a natural treatment to schistosomiasis.</p>
      </Card>
      </div>
  );
}
```

