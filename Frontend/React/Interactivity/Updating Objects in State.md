State can hold any kind of JavaScript value, including objects. But you shouldn't change objects that you hold in the React state directly. Instead, when you want to update an object, you need to create a new one (or make a copy of an existing one), and then set the state to use that copy.
### What's a mutation?

You can store any kind of JavaScript value in state.

```jsx
const [x, setX] = useState(0);
```

So far you've been working with numbers, strings and booleans. These kinds of JavaScript values are "immutable", meaning unchangeable or "read-only". You can trigger a re-render to replace a value:

```jsx
setX(5);
```

The `x` state changed from `0` to `5`, but the _number `0` itself_ did not change. It’s not possible to make any changes to the built-in primitive values like numbers, strings, and booleans in JavaScript.

Now consider an object in state:

```jsx
const [position, setPosition] = useState({ x: 0, y: 0 });
```

```jsx
position.x = 5;
```

However, although objects in React state are technically mutable, you should treat them **as if** they were immutable—like numbers, booleans, and strings. Instead of mutating them, you should always replace them.

### Treat state as read-only.

In other words, you should **treat any JavaScript object that you put into state as read-only.**

This example holds an object in state to represent the current pointer position. The red dot is supposed to move when you touch or move the cursor over the preview area. But the dot stays in the initial position:

```jsx
import { useState } from 'react';

export default function MovingDot() {
  const [position, setPosition] = useState({
    x: 0,
    y: 0
  });
  return (
    <div
      onPointerMove={e => {
        setPosition({
          x : e.clientX,
          y : e.clientY
        })
      }}
      style={{
        position: 'relative',
        width: '100vw',
        height: '100vh',
      }}>
      <div style={{
        position: 'absolute',
        backgroundColor: 'red',
        borderRadius: '50%',
        transform: `translate(${position.x}px, ${position.y}px)`,
        left: -10,
        top: -10,
        width: 20,
        height: 20,
      }} />
    </div>
  );
}
```

The problem is with this bit of code.

```jsx
onPointerMove={e => {  
	position.x = e.clientX;  
	position.y = e.clientY;  
}}
```

This code modifies the object assigned to `position` from [the previous render.](https://react.dev/learn/state-as-a-snapshot#rendering-takes-a-snapshot-in-time) But without using the state setting function, React has no idea that object has changed. So React does not do anything in response. It’s like trying to change the order after you’ve already eaten the meal. While mutating state can work in some cases, we don’t recommend it. You should treat the state value you have access to in a render as read-only.

To actually [trigger a re-render](https://react.dev/learn/state-as-a-snapshot#setting-state-triggers-renders) in this case, **create a _new_ object and pass it to the state setting function:**

```jsx
onPointerMove={e => {  
	setPosition({  
		x: e.clientX,  
		y: e.clientY  
	});  
}}
```

With `setPosition`, you’re telling React:

- Replace `position` with this new object
- And render this component again

Notice how the red dot now follows your pointer when you touch or hover over the preview area.

#### Local mutation is fine.

Code like this is a problem because it modifies an _existing_ object in state:

```jsx
position.x = e.clientX;  
position.y = e.clientY;
```

But code like this is **absolutely fine** because you’re mutating a fresh object you have _just created_:

```jsx
const nextPosition = {};  
nextPosition.x = e.clientX;  
nextPosition.y = e.clientY;  
setPosition(nextPosition);
```

In fact, it is completely equivalent to writing this:

```jsx
setPosition({  
	x: e.clientX,  
	y: e.clientY  
});
```

Mutation is only a problem when you change _existing_ objects that are already in state. Mutating an object you’ve just created is okay because _no other code references it yet._ Changing it isn’t going to accidentally impact something that depends on it. This is called a “local mutation”. You can even do local mutation [while rendering.](https://react.dev/learn/keeping-components-pure#local-mutation-your-components-little-secret) Very convenient and completely okay!
### Copying objects with the spread syntax.

In the previous example, the `position` object is always created fresh from the current cursor position. But often, you will want to include existing data as a part of the new object you're creating. For example, you may want to update only one field in a form, but keep the previous values of all other fields.

These input fields don't work because the `onChange` handlers mutate the state:

```jsx
person.firstName = e.target.value;
```

The reliable way to get the behavior you’re looking for is to create a new object and pass it to `setPerson`. But here, you want to also **copy the existing data into it** because only one of the fields has changed:

```jsx
setPerson({  
	firstName: e.target.value, // New first name from the input  
	lastName: person.lastName,  
	email: person.email  
});
```

You can use the `...` [object spread](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#spread_in_object_literals) syntax so that you don’t need to copy every property separately.

```jsx
setPerson({  
	...person, // Copy the old fields  
	firstName: e.target.value // But override this one  
});
```

```jsx
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });

  function handleFirstNameChange(e) {
    setPerson({
      ...person,
      firstName : e.target.value
    }); 
  }

  function handleLastNameChange(e) {
    setPerson({
      ...person,
      lastName : e.target.value
    });
  }

  function handleEmailChange(e) {
    setPerson({
      ...person,
      email : e.target.value
    });
  }

  return (
    <>
      <label>
        First name:
        <input
          value={person.firstName}
          onChange={handleFirstNameChange}
        />
      </label>
      <label>
        Last name:
        <input
          value={person.lastName}
          onChange={handleLastNameChange}
        />
      </label>
      <label>
        Email:
        <input
          value={person.email}
          onChange={handleEmailChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```

Note that the `...` spread syntax is “shallow”—it only copies things one level deep. This makes it fast, but it also means that if you want to update a nested property, you’ll have to use it more than once.
#### Using a single event handler for multiple fields.

You can also use the `[` and `]` braces inside your object definition to specify a property with a dynamic name. Here is the same example, but with a single event handler instead of three different ones:

```jsx
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });

  function handleChange(e) {
    setPerson({
      ...person,
      [e.target.name]: e.target.value
    });
  }

  return (
    <>
      <label>
        First name:
        <input
          name="firstName"
          value={person.firstName}
          onChange={handleChange}
        />
      </label>
      <label>
        Last name:
        <input
          name="lastName"
          value={person.lastName}
          onChange={handleChange}
        />
      </label>
      <label>
        Email:
        <input
          name="email"
          value={person.email}
          onChange={handleChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```

Here, `e.target.name` refers to the `name` property given to the `<input>` DOM element.

### Updating a nested object.

Consider a nested object structure like this:

```jsx
const [person, setPerson] = useState({  
	name: 'Niki de Saint Phalle',  
	artwork: {  
		title: 'Blue Nana',  
		city: 'Hamburg',  
		image: 'https://i.imgur.com/Sd1AgUOm.jpg',  
}  
});
```

```jsx
person.artwork.city = 'New Delhi';
```

But in React, you treat state as immutable! In order to change `city`, you would first need to produce the new `artwork` object (pre-populated with data from the previous one), and then produce the new `person` object which points at the new `artwork`:

```jsx
const nextArtwork = { ...person.artwork, city: 'New Delhi' };  
const nextPerson = { ...person, artwork: nextArtwork };  
setPerson(nextPerson);
```

Or, written as a single function call:

```jsx
setPerson({  
	...person, // Copy other fields  
	artwork: { // but replace the artwork  
		...person.artwork, // with the same one  
		city: 'New Delhi' // but in New Delhi!  
	}  
});
```

#### Objects are not really nested

An object like this appears “nested” in code:

```jsx
let obj = {  
	name: 'Niki de Saint Phalle',  
	artwork: {  
		title: 'Blue Nana',  
		city: 'Hamburg',  
		image: 'https://i.imgur.com/Sd1AgUOm.jpg',  
	}  
};
```

However, “nesting” is an inaccurate way to think about how objects behave. When the code executes, there is no such thing as a “nested” object. You are really looking at two different objects:

```jsx
let obj1 = {  
	title: 'Blue Nana',  
	city: 'Hamburg',  
	image: 'https://i.imgur.com/Sd1AgUOm.jpg',  
};  

let obj2 = {  
	name: 'Niki de Saint Phalle',  
	artwork: obj1  
};
```

The `obj1` object is not “inside” `obj2`. For example, `obj3` could “point” at `obj1` too:

```jsx
let obj1 = {  
	title: 'Blue Nana',  
	city: 'Hamburg',  
	image: 'https://i.imgur.com/Sd1AgUOm.jpg'  
};  

let obj2 = {  
	name: 'Niki de Saint Phalle',  
	artwork: obj1  
};  

let obj3 = {  
	name: 'Copycat',  
	artwork: obj1  
};
```

If you were to mutate `obj3.artwork.city`, it would affect both `obj2.artwork.city` and `obj1.city`. This is because `obj3.artwork`, `obj2.artwork`, and `obj1` are the same object. This is difficult to see when you think of objects as “nested”. Instead, they are separate objects “pointing” at each other with properties.

#### Write concise update logic with Immer.

If your state is deeply nested, you might want to consider [flattening it.](https://react.dev/learn/choosing-the-state-structure#avoid-deeply-nested-state) But, if you don’t want to change your state structure, you might prefer a shortcut to nested spreads. [Immer](https://github.com/immerjs/use-immer) is a popular library that lets you write using the convenient but mutating syntax and takes care of producing the copies for you. With Immer, the code you write looks like you are “breaking the rules” and mutating an object:

```jsx
updatePerson(draft => {  
	draft.artwork.city = 'Lagos';  
});
```

#### How does Immer work?

The `draft` provided by Immer is a special type of object, called a [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy), that “records” what you do with it. This is why you can mutate it freely as much as you like! Under the hood, Immer figures out which parts of the `draft` have been changed, and produces a completely new object that contains your edits.

To try Immer:

1. Run `npm install use-immer` to add Immer as a dependency
2. Then replace `import { useState } from 'react'` with `import { useImmer } from 'use-immer'`

```jsx
import { useImmer } from 'use-immer';

export default function Form() {
  const [person, updatePerson] = useImmer({
    name: 'Niki de Saint Phalle',
    artwork: {
      title: 'Blue Nana',
      city: 'Hamburg',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleNameChange(e) {
    updatePerson(draft => {
      draft.name = e.target.value;
    });
  }

  function handleTitleChange(e) {
    updatePerson(draft => {
      draft.artwork.title = e.target.value;
    });
  }

  function handleCityChange(e) {
    updatePerson(draft => {
      draft.artwork.city = e.target.value;
    });
  }

  function handleImageChange(e) {
    updatePerson(draft => {
      draft.artwork.image = e.target.value;
    });
  }

  return (
    <>
      <label>
        Name:
        <input
          value={person.name}
          onChange={handleNameChange}
        />
      </label>
      <label>
        Title:
        <input
          value={person.artwork.title}
          onChange={handleTitleChange}
        />
      </label>
      <label>
        City:
        <input
          value={person.artwork.city}
          onChange={handleCityChange}
        />
      </label>
      <label>
        Image:
        <input
          value={person.artwork.image}
          onChange={handleImageChange}
        />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {' by '}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>
      <img 
        src={person.artwork.image} 
        alt={person.artwork.title}
      />
    </>
  );
}
```

Notice how much more concise the event handlers have become. You can mix and match `useState` and `useImmer` in a single component as much as you like. Immer is a great way to keep the update handlers concise, especially if there’s nesting in your state, and copying objects leads to repetitive code.
#### Why is mutating state not recommended in React? 

There are a few reasons:

- **Debugging:** If you use `console.log` and don’t mutate state, your past logs won’t get clobbered by the more recent state changes. So you can clearly see how state has changed between renders.
- **Optimizations:** Common React [optimization strategies](https://react.dev/reference/react/memo) rely on skipping work if previous props or state are the same as the next ones. If you never mutate state, it is very fast to check whether there were any changes. If `prevObj === obj`, you can be sure that nothing could have changed inside of it.
- **New Features:** The new React features we’re building rely on state being [treated like a snapshot.](https://react.dev/learn/state-as-a-snapshot) If you’re mutating past versions of state, that may prevent you from using the new features.
- **Requirement Changes:** Some application features, like implementing Undo/Redo, showing a history of changes, or letting the user reset a form to earlier values, are easier to do when nothing is mutated. This is because you can keep past copies of state in memory, and reuse them when appropriate. If you start with a mutative approach, features like this can be difficult to add later on.
- **Simpler Implementation:** Because React does not rely on mutation, it does not need to do anything special with your objects. It does not need to hijack their properties, always wrap them into Proxies, or do other work at initialization as many “reactive” solutions do. This is also why React lets you put any object into state—no matter how large—without additional performance or correctness pitfalls.

In practice, you can often “get away” with mutating state in React, but we strongly advise you not to do that so that you can use new React features developed with this approach in mind. Future contributors and perhaps even your future self will thank you!


### Recap

- Treat all state in React as immutable.
- When you store objects in state, mutating them will not trigger renders and will change the state in previous render “snapshots”.
- Instead of mutating an object, create a _new_ version of it, and trigger a re-render by setting state to it.
- You can use the `{...obj, something: 'newValue'}` object spread syntax to create copies of objects.
- Spread syntax is shallow: it only copies one level deep.
- To update a nested object, you need to create copies all the way up from the place you’re updating.
- To reduce repetitive copying code, use Immer.

### Challenges

#### 1. Fix incorrect state updates

This form has a few bugs. Click the button that increases the score a few times. Notice that it does not increase. Then edit the first name, and notice that the score has suddenly “caught up” with your changes. Finally, edit the last name, and notice that the score has disappeared completely.

```jsx
import { useState } from 'react';

export default function Scoreboard() {
  const [player, setPlayer] = useState({
    firstName: 'Ranjani',
    lastName: 'Shettar',
    score: 10,
  });

  function handlePlusClick() {
    setPlayer({
      ...player,
      score : player.score + 1
    })
  }

  function handleFirstNameChange(e) {
    setPlayer({
      ...player,
      firstName: e.target.value,
    });
  }

  function handleLastNameChange(e) {
    setPlayer({
      ...player,
      lastName: e.target.value
    });
  }

  return (
    <>
      <label>
        Score: <b>{player.score}</b>
        {' '}
        <button onClick={handlePlusClick}>
          +1
        </button>
      </label>
      <label>
        First name:
        <input
          value={player.firstName}
          onChange={handleFirstNameChange}
        />
      </label>
      <label>
        Last name:
        <input
          value={player.lastName}
          onChange={handleLastNameChange}
        />
      </label>
    </>
  );
}
```

#### 2. [Find and fix the mutation](https://react.dev/learn/updating-objects-in-state#find-and-fix-the-mutation)

```jsx
import { useState } from 'react';
import Background from './Background.js';
import Box from './Box.js';

const initialPosition = {
  x: 0,
  y: 0
};

export default function Canvas() {
  const [shape, setShape] = useState({
    color: 'orange',
    position: initialPosition
  });

  function handleMove(dx, dy) {
    setShape({
      ...shape,
      position : {
        x: shape.position.x + dx,
        y: shape.position.y + dy
      }
    });
  }

  function handleColorChange(e) {
    setShape({
      ...shape,
      color: e.target.value
    });
  }

  return (
    <>
      <select
        value={shape.color}
        onChange={handleColorChange}
      >
        <option value="orange">orange</option>
        <option value="lightpink">lightpink</option>
        <option value="aliceblue">aliceblue</option>
      </select>
      <Background
        position={initialPosition}
      />
      <Box
        color={shape.color}
        position={shape.position}
        onMove={handleMove}
      >
        Drag me!
      </Box>
    </>
  );
}
```

#### 3. Update an object with Immer

This is the same buggy example as in the previous challenge. This time, fix the mutation by using Immer. For your convenience, `useImmer` is already imported, so you need to change the `shape` state variable to use it.

```jsx
import { useState } from 'react';
import { useImmer } from 'use-immer';
import Background from './Background.js';
import Box from './Box.js';

const initialPosition = {
  x: 0,
  y: 0
};

export default function Canvas() {
  const [shape, setShape] = useImmer({
    color: 'orange',
    position: initialPosition
  });

  function handleMove(dx, dy) {
    setShape(s => {
      s.position.x += dx;
      s.position.y += dy;
    })
  }

  function handleColorChange(e) {
    setShape(s => {
      s: e.target.value;
    });
  }

  return (
    <>
      <select
        value={shape.color}
        onChange={handleColorChange}
      >
        <option value="orange">orange</option>
        <option value="lightpink">lightpink</option>
        <option value="aliceblue">aliceblue</option>
      </select>
      <Background
        position={initialPosition}
      />
      <Box
        color={shape.color}
        position={shape.position}
        onMove={handleMove}
      >
        Drag me!
      </Box>
    </>
  );
}
```

