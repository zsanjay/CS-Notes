_JSX_ is a syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file. Although there are other ways to write components, most React developers prefer the conciseness of JSX, and most codebases use it.
### JSX: Putting markup into JavaScript

The Web has been built on HTML, CSS, and JavaScript. For many years, web developers kept content in HTML, design in CSS, and logic in JavaScript—often in separate files! Content was marked up inside HTML while the page’s logic lived separately in JavaScript:

But as the Web became more interactive, logic increasingly determined content. JavaScript was in charge of the HTML! This is why **in React, rendering logic and markup live together in the same place—components.**

Each React component is a JavaScript function that may contain some markup that React renders into the browser. React components use a syntax extension called JSX to represent that markup. JSX looks a lot like HTML, but it is a bit stricter and can display dynamic information. The best way to understand this is to convert some HTML markup to JSX markup.
#### Note

JSX and React are two separate things. They’re often used together, but you _can_ [use them independently](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html#whats-a-jsx-transform) of each other. JSX is a syntax extension, while React is a JavaScript library.
#### Converting HTML to JSX.

HTML

```jsx
<h1>Hedy Lamarr's Todos</h1>  

<img  
	src="https://i.imgur.com/yXOvdOSs.jpg"  
	alt="Hedy Lamarr"  
	class="photo"  
>  

<ul>  
	<li>Invent new traffic lights  
	<li>Rehearse a movie scene  
	<li>Improve the spectrum technology  
</ul>
```

TodoList Component

```jsx
export default function TodoList() {
  return (
    // This doesn't quite work!
    <h1>Hedy Lamarr's Todos</h1>
    <img 
      src="https://i.imgur.com/yXOvdOSs.jpg" 
      alt="Hedy Lamarr" 
      class="photo"
    >
    <ul>
      <li>Invent new traffic lights
      <li>Rehearse a movie scene
      <li>Improve the spectrum technology
    </ul>
  );
}
```

The above component will not work and give you an error.

```
/src/App.js: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>?
```

This is because JSX is stricter and has a few more rules than HTML! If you read the error messages above, they’ll guide you to fix the markup, or you can follow the guide below.

#### Note

Most of the time, React’s on-screen error messages will help you find where the problem is. Give them a read if you get stuck!

### The Rules of JSX

#### 1. Return a single root element

To return multiple elements from a component, **wrap them with a single parent tag.**

For example, you can use a `<div>`:

```jsx
<div>  
<h1>Hedy Lamarr's Todos</h1>  
<img  
	src="https://i.imgur.com/yXOvdOSs.jpg"  
	alt="Hedy Lamarr"  
	class="photo">  
<ul>  
</ul>  
</div>
```

If you don’t want to add an extra `<div>` to your markup, you can write `<>` and `</>` instead:

```jsx
<>  
<h1>Hedy Lamarr's Todos</h1>  
<img  
	src="https://i.imgur.com/yXOvdOSs.jpg"  
	alt="Hedy Lamarr"  
	class="photo">  
<ul>  
</ul>  
</>
```

This empty tag is called a _[Fragment.](https://react.dev/reference/react/Fragment)_ Fragments let you group things without leaving any trace in the browser HTML tree.
#### Why do multiple JSX tags need to be wrapped? 

JSX looks like HTML, but under the hood it is transformed into plain JavaScript objects. You can’t return two objects from a function without wrapping them into an array. This explains why you also can’t return two JSX tags without wrapping them into another tag or a Fragment.

#### 2. Close all the tags.

JSX requires tags to be explicitly closed: self-closing tags like `<img>` must become `<img />`, and wrapping tags like `<li>oranges` must be written as `<li>oranges</li>`.

#### 3. camelCase ~~all~~ most of the things!

JSX turns into JavaScript and attributes written in JSX become keys of JavaScript objects. In your own components, you will often want to read those attributes into variables. But JavaScript has limitations on variable names. For example, their names can’t contain dashes or be reserved words like `class`.

This is why, in React, many HTML and SVG attributes are written in camelCase. For example, instead of `stroke-width` you use `strokeWidth`. Since `class` is a reserved word, in React you write `className` instead, named after the [corresponding DOM property](https://developer.mozilla.org/en-US/docs/Web/API/Element/className):

```jsx
<img  
	src="https://i.imgur.com/yXOvdOSs.jpg"  
	alt="Hedy Lamarr"  
	className="photo"  
/>
```

You can [find all these attributes in the list of DOM component props.](https://react.dev/reference/react-dom/components/common) If you get one wrong, don’t worry—React will print a message with a possible correction to the [browser console.](https://developer.mozilla.org/docs/Tools/Browser_Console)

#### Pro-tip: Use a JSX Converter 

Converting all these attributes in existing markup can be tedious! We recommend using a [converter](https://transform.tools/html-to-jsx) to translate your existing HTML and SVG to JSX. Converters are very useful in practice, but it’s still worth understanding what is going on so that you can comfortably write JSX on your own.
### Recap

Now you know why JSX exists and how to use it in components:

- React components group rendering logic together with markup because they are related.
- JSX is similar to HTML, with a few differences. You can use a [converter](https://transform.tools/html-to-jsx) if you need to.
- Error messages will often point you in the right direction to fixing your markup.

```jsx
export default function Bio() {
  return (
    <>
    <div className="intro">
      <h1>Welcome to my website!</h1>
    </div>
    <p className="summary">
      You can find my thoughts here.
      <br/><br/>
      <b>And <i>pictures</i> </b>of scientists!
    </p>
    </>
  );
}
```

