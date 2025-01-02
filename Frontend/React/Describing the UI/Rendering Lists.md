You will often want to display multiple similar components from a collection of data. You can use the [JavaScript array methods](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array#) to manipulate an array of data. On this page, you’ll use [`filter()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) and [`map()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/map) with React to filter and transform your array of data into an array of components.

### Rendering data from arrays.

```html
<ul>  
	<li>Creola Katherine Johnson: mathematician</li>  
	<li>Mario José Molina-Pasquel Henríquez: chemist</li>  
	<li>Mohammad Abdus Salam: physicist</li>  
	<li>Percy Lavon Julian: chemist</li>  
	<li>Subrahmanyan Chandrasekhar: astrophysicist</li>  
</ul>
```

1. **Move** the data into an array:

```js
const people = [  
'Creola Katherine Johnson: mathematician',  
'Mario José Molina-Pasquel Henríquez: chemist',  
'Mohammad Abdus Salam: physicist',  
'Percy Lavon Julian: chemist',  
'Subrahmanyan Chandrasekhar: astrophysicist'  
];
```

2. **Map** the `people` members into a new array of JSX nodes, `listItems`:

```jsx
const listItems = people.map(person => <li>{person}</li>);
```

3. **Return** `listItems` from your component wrapped in a `<ul>`:

```jsx
return <ul>{listItems}</ul>;
```

```jsx
export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```

Warning: Each child in a list should have a unique “key” prop.

### Filtering arrays of items

```js
const people = [{  
	id: 0,  
	name: 'Creola Katherine Johnson',  
	profession: 'mathematician',  
}, {  
	id: 1,  
	name: 'Mario José Molina-Pasquel Henríquez',  
	profession: 'chemist',  
}, {  
	id: 2,  
	name: 'Mohammad Abdus Salam',  
	profession: 'physicist',  
}, {  
	id: 3,  
	name: 'Percy Lavon Julian',  
	profession: 'chemist',  
}, {  
	id: 4,  
	name: 'Subrahmanyan Chandrasekhar',  
	profession: 'astrophysicist',  
}];
```

Let’s say you want a way to only show people whose profession is `'chemist'`. You can use JavaScript’s `filter()` method to return just those people. This method takes an array of items, passes them through a “test” (a function that returns `true` or `false`), and returns a new array of only those items that passed the test (returned `true`).

You only want the items where `profession` is `'chemist'`. The “test” function for this looks like `(person) => person.profession === 'chemist'`. Here’s how to put it together:

1. **Create** a new array of just “chemist” people, `chemists`, by calling `filter()` on the `people` filtering by `person.profession === 'chemist'`:

```jsx
const chemists = people.filter(person =>  
	person.profession === 'chemist'  
);
```

#### App.js

```jsx
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
 // 1. Filtering
  const chemists = people.filter(person =>
    person.profession === 'chemist'
  );

   //2. Now map over `chemists`:
  const listItems = chemists.map(person =>
    <li>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );

 // 3. Lastly, return the `listItems` from your component:
  return <ul>{listItems}</ul>;
}
```

#### Pitfall

Arrow functions implicitly return the expression right after `=>`, so you didn’t need a `return` statement:

```jsx
const listItems = chemists.map(person =>  <li>...</li> // Implicit return!  
);
```

However, **you must write `return` explicitly if your `=>` is followed by a `{` curly brace!**

```jsx
const listItems = chemists.map(person => { // Curly brace  
	return <li>...</li>;  
});
```

Arrow functions containing `=> {` are said to have a [“block body”.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#function_body) They let you write more than a single line of code, but you _have to_ write a `return` statement yourself. If you forget it, nothing gets returned!

#### Displaying several DOM nodes for each list item.

What do you do when each item needs to render not one, but several DOM nodes?

The short [`<>...</>` Fragment](https://react.dev/reference/react/Fragment) syntax won’t let you pass a key, so you need to either group them into a single `<div>`, or use the slightly longer and [more explicit `<Fragment>` syntax:](https://react.dev/reference/react/Fragment#rendering-a-list-of-fragments)

```jsx
import { Fragment } from 'react';  

// ...  

const listItems = people.map(person =>  
	<Fragment key={person.id}>  
		<h1>{person.name}</h1>  
		<p>{person.bio}</p>  
	</Fragment>  
);
```

Fragments disappear from the DOM, so this will produce a flat list of `<h1>`, `<p>`, `<h1>`, `<p>`, and so on.

### Challenges

#### Splitting a list in two

```jsx
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const chemists = people.filter((person) => person.profession === 'chemist');
  const notChemists = people.filter((person) => person.profession !== 'chemist');
  
  const chemistItems = chemists.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );

  const notChemistItems = notChemists.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );
  return (
    <article>
      <h1>Scientists</h1>
      <h2>Chemists</h2>
      <ul>{chemistItems}</ul>
      <h2>Everyone Else</h2>
      <ul>{notChemistItems}</ul>
    </article>
  );
}
```

#### Nested lists in one component

```jsx
import { recipes } from './data.js';

export default function RecipeList() {
  return (
    <div>
      <h1>Recipes</h1>
      {recipes.map(recipe => 
        <div key={recipe.id}>
        <h2>{recipe.name}</h2>
        <ul>
          {recipe.ingredients.map((ingredient) => {
              return <li key={ingredient}>
                {ingredient}
              </li>;
            })
          }
        </ul>
        </div>
      )}
    </div>
  );
}
```

#### Extracting a list item component

```jsx
import { recipes } from './data.js';

function Recipe({id , name , ingredients}) {
  return (
        <div>
          <h2>{name}</h2>
          <ul>
            {ingredients.map(ingredient =>
              <li key={ingredient}>
                {ingredient}
              </li>
            )}
          </ul>
      </div>
  );
}

export default function RecipeList() {
  return (
    <div>
      <h1>Recipes</h1>
      {recipes.map(recipe =>
        <Recipe {...recipe} key={recipe.id}/>
      )}
  </div>
  );
}
```

#### List with a separator

1. Using Array

Using the original line index as a `key` doesn’t work anymore because each separator and paragraph are now in the same array. However, you can give each of them a distinct key using a suffix, e.g. `key={i + '-text'}`.

```jsx
const poem = {
  lines: [
    'I write, erase, rewrite',
    'Erase again, and then',
    'A poppy blooms.'
  ]
};

export default function Poem() {
  let output = [];

  // Fill the output array
  poem.lines.forEach((line, i) => {
    output.push(
      <hr key={i + '-separator'} />
    );
    output.push(
      <p key={i + '-text'}>
        {line}
      </p>
    );
  });
  // Remove the first <hr />
  output.shift();

  return (
    <article>
      {output}
    </article>
  );
}
```

2. Using Fragment

Remember, Fragments (often written as `<> </>`) let you group JSX nodes without adding extra `<div>`s!

```jsx
import { Fragment } from 'react';

const poem = {
  lines: [
    'I write, erase, rewrite',
    'Erase again, and then',
    'A poppy blooms.'
  ]
};

export default function Poem() {
  return (
    <article>
      {poem.lines.map((line, i) =>
        <Fragment key={i}>
          {i > 0 && <hr />}
          <p>{line}</p>
        </Fragment>
      )}
    </article>
  );
}

```



