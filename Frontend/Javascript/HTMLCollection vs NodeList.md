
## What is an `HTMLCollection`?

An `HTMLCollection` is a list of DOM elements that match certain criteria. For example, they may have the same tag name or class. Or they may be related in a specific context, like children of a particular element. 

Here's an example:

```html
  <button class="btn">First button</button>
  <button class="btn">Second button</button>
  <button class="btn">Third button</button>
```

In this example, we have three button elements. Each has a class of `btn`. Now, let's select the buttons using the `getElementsByClassName` method.

```javascript
const buttonElements = document.getElementsByClassName('btn')
console.log(buttonElements)
```

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-04-at-8.10.41-AM.png)

The `getElementsByClassName` methods returns an `HTMLCollection` of the three buttons with the `btn` class. It looks like an array but it's not. More on that later.

## What is a `NodeList`?

Like the name suggests, a `NodeList` is a list of nodes. But what is a node? A node is any individual element in the DOM tree. This could be elements, attributes, texts, comments, and so on.

An example of a DOM method that will return a `nodeList` is `querySelectorAll`.

Example:

```html
  <button class="btn">First button</button>
  <button class="btn">Second button</button>
  <button class="btn">Third button</button>
```

Using the same example, let's select the buttons with `querySelectorAll`instead.

```javascript
const buttonElements = document.querySelectorAll('.btn')
console.log(buttonElements)
```

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-04-at-8.33.08-AM.png)

This time, the `console.log` statement prints a `NodeList`. Again, this is also similar to an array but it isn't quite an array.

## Similarities between `HTMLCollection` and `NodeList`

Now that you know what `HTMLCollection` and `NodeList` are, let's look at how they are alike. They both aren't arrays even though they look like one. But they have features that make them have some behaviours of arrays.

You can access the contents of both using zero-based indexing like you would with an array. And you can also use the length property to find the length of both an `HTMLCollection` and a `NodeList`.

Example:

```html
  <div>
    <p class="paragraph">First paragraph</p>
    <p class="paragraph">Second paragraph</p>
    <p class="paragraph">Third paragraph</p>
  </div>
```

This is a `div` with three paragraphs. Let's see examples of accessing the elements with zero-based indexing and also checking the length for both `HTMLCollection` and `NodeList`.

### Example with `HTMLCollection`:

```javascript
// getElementsByClassName will return an HTMLCollection
const paragraphs = document.getElementsByClassName("paragraph")
console.log(paragraphs)

// Use the index to get the first paragraph
let firstParagraph = paragraphs[0] 
console.log(firstParagraph)

// Use the length property
console.log(paragraphs.length)
```

The screenshot below shows the results for the three `console.log`statements.

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-04-at-9.38.09-AM.png)

Even though the `HTMLCollection` is not an array, you can still use the index to access the items in the collection. And you can also get the length using the `length` property.

You will get the same result for a `NodeList`. To get a `NodeList`, let's use the `querySelectorAll` method instead.

### Example with `NodeList`:

```javascript
// querySelectorAll will return a Nodelist
const paragraphs = document.querySelectorAll(".paragraph")
console.log(paragraphs)

// Use the index to get the first paragraph
let firstParagraph = paragraphs[0] 
console.log(firstParagraph)

// Use the length property
console.log(paragraphs.length)
```

Just like the `HTMLCollection`, the `NodeList` also uses zero-based indexing. And you can also use the length property on it.

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-04-at-9.55.22-AM.png)

## Differences between `HTMLCollection` and `NodeList`

You've seen how an `HTMLCollection` and a `NodeList` are alike. But there are also some differences you need to be aware of when working with these two types of collections in the DOM.

### Elements nodes only vs all nodes

Elements nodes are HTML elements like `<p>`, `<div>`, `<img>`, and others. But there are other types of nodes too. For example, text nodes and attribute nodes.

An `HTMLCollection` will include only element nodes whiles a `NodeList`includes other node types.

Example:

```javascript
<div>
  This is a text
  <p class="paragraph">First paragraph</p>
  <p class="paragraph">First paragraph</p>
</div>
```

Here is a `div` with a text node and two element nodes (paragraphs). Each paragraph also has a text node. 

Assuming you wanted to get only the element nodes in the `div`, you can use the `children` property on the `div`. And it will return an `HTMLCollection` containing only the element nodes.

But if you wanted all the nodes and not just the element nodes, then you can use the `childNodes` property to get all the nodes.

```javascript
const divElement = document.querySelector('div')

console.log(divElement.children) // returns an HTMLCollection
console.log(divElement.childNodes) // returns a NodeList
```

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-04-at-10.59.18-AM.png)

The `HTMLCollection` has two items: the paragraph element nodes. Whilst the `NodeList` includes the first text and the two paragraphs and their text contents too.

### Live Collections vs Static Collections

The concepts of "live" and "static" refer to how an `HTMLCollection` and `NodeList` behave in response to changes in the document structure.

#### An `HTMLCollection` is always live

What does it mean to say an `HTMLCollection` is always live? It means when there is a change in the document, it will be automatically updated to reflect the change.

Example:

```html
<p>Paragraph One</p>
<p>Paragraph Two</p>
<p>Paragraph Three</p>
```

```javascript
// returns an HTMLCollection
const paragraphs = document.getElementsByTagName('p')

console.log("BEFORE UPDATE: ", paragraphs)

const newParagraph = document.createElement('p')
document.body.appendChild(newParagraph)

console.log("AFTER UPDATE: ", paragraphs)
```

The code above creates an `HTMLCollection` called `paragraphs` using the `getElementsByTagName` method. And there are two `console.log`statements. One before a new paragraph is created and appended to the body, and another one after that.

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-06-at-9.04.10-AM.png)

Before the update, the `HTMLCollection` had three elements. But after the update, the collection now has four elements, reflecting the change in the document.

#### A `NodeList` is sometimes static

A `NodeList` is not always live. It can be static or live depending on how it is generated. For example, a `NodeList` generated with the `querySelectorAll`method is static. A change in the document isn't reflected in the `NodeList`.

Example:

```html
<p>Paragraph One</p>
<p>Paragraph Two</p>
<p>Paragraph Three</p>
```

```javascript
// returns an HTMLCollection
const paragraphs = document.getElementsByTagName('p')

console.log("BEFORE UPDATE: ", paragraphs)

const newParagraph = document.createElement('p')
document.body.appendChild(newParagraph)

console.log("AFTER UPDATE: ", paragraphs)
```

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-06-at-9.09.42-AM.png)

Because of the static nature of the `NodeList`, it remains the same even after an update in the document.

Note: in exceptional cases, like when a `NodeList` is generated with the `getElementsByName`, that `NodeList` will be live.

### How to Access Items in the Collection

When accessing elements in an `HTMLCollection`, you can use any of the following. 

- The index of element. 
- Their `id` attribute with the `namedItem` property. 
- Their `name` attribute with the `namedItem` property. 

But with a `NodeList`, you can only access the nodes in the list only by their index.

```javascript
<div id="container">
  <button id="btn1" name="first-name">First Button</button>
  <button id="btn2">Second Button</button>
  <button id="btn3">Third Button</button>
</div>
```

Here is a `div` container with three buttons. Note the first button has an id attribute and a name attribute.

#### Example with `HTMLCollection`:

```javascript
const container = document.querySelector('#container')
const buttons = container.children // returns HTMLCollection

console.log(buttons[0])// using the index
console.log(buttons.namedItem("btn1")) // using the id attribute
console.log(buttons.namedItem("first-name")) // using the name attribute
```

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-04-at-11.57.33-AM.png)

All three `console.log`s successfully return the first button.

#### Example with `NodeList`:

```javascript
const container = document.querySelector('#container')
const buttons = container.childNodes // returns a NodeList

console.log(buttons[1])// using the index
console.log(buttons.namedItem("btn1")) // throws an error
console.log(buttons.namedItem("first-name")) // throws an error
```

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-06-at-7.19.56-AM.png)

Using the same example for a `NodeList`, the first `console.log` statement prints the button. But the other two raise a `TypeError`.

### How to loop through the collection

You cannot loop through an `HTMLCollection` with any of the array methods. Unless you first create an array from the collection. 

But with a `NodeList`, you can use the `forEach` method to loop through it. But you cannot use other array methods like `map`, `filter`, and others without first creating an array from it.

Example:

```html
<button class="btn">First button</button>
<button class="btn">Second button</button>
<button class="btn">Third button</button>
```

The code below attempts to loop through an `HTMLCollection` with the `forEach` method and results in an `TypError`.

```javascript
// returns an HTMLCollection
const allButtons = document.getElementsByClassName('btn') 

allButtons.forEach(button => console.log(button))
```

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-06-at-8.04.26-AM.png)

Let's see another example but with a `NodeList`.

```javascript
// returns a NodeList
const allButtons = document.querySelectorAll('.btn') 

allButtons.forEach(button => console.log(button))
```

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-06-at-8.07.27-AM.png)

In the example above, the `forEach` method works successfully on the `NodeList`.

If for some reason, you still want to loop through an `HTMLCollection`without first creating an array from it, you can use the `for...of` statement. Let's use the same buttons example to show how you can do that.

```javascript
// returns an HTMLCollection
const allButtons = document.getElementsByClassName('btn')

for (button of allButtons) {
  console.log(button)
}
```

![Image](https://www.freecodecamp.org/news/content/images/2023/12/Screenshot-2023-12-06-at-8.07.27-AM-1.png)

## Which One Should You Use?

The question of whether you should use an `HTMLCollection` or a `NodeList`depends on the use case or specific context. 

If you want a live collection that automatically updates when there's a change in the document, then you should use an `HTMLCollection`. But if you prefer a static collection that doesn't update with a change in the document, then you should use a `NodeList`.

Most modern JavaScript frameworks and libraries provide higher-level abstractions, simplifying many DOM manipulation tasks. And you don't need to worry about them.

But having a solid understanding of native DOM collections like `HTMLCollection` and `NodeList` remains valuable, especially in scenarios where fine-grained control or compatibility with legacy code is essential.

## Conclusion

In this article you learned about `HTMLCollection` and `NodeList`. You learned about what they are, their similarities and differences. The article also touched on when you should consider using either an `HTMLCollection`or a `NodeList`.


### HTMLCollection

https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection

### Node Type

https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType


![[20241225191436.png]]


### References

https://www.freecodecamp.org/news/dom-manipulation-htmlcollection-vs-nodelist/#heading-what-is-an-htmlcollection

https://www.youtube.com/watch?v=uwJyp4ZLVMA

