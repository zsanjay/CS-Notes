
Martin Fowler has the feeling that loops are an outdated concept. He already mentioned them as an issue in his first edition of "Refactoring: Improving the design of existing code" book, although there were no better alternatives at that time. [[1](https://luzkan.github.io/smells/imperative-loops#sources)] Nowadays, languages provide an alternative - pipelines. Fowler, in his 2018 book (third edition), suggests that anachronistic loops should be replaced by pipeline operations such as `filter`, `map`, or `reduce` [[2](https://luzkan.github.io/smells/imperative-loops#sources)].

Indeed, loops can sometimes be hard to read and error-prone. This might be unconfirmed, but I doubt the existence of a programmer who has never had an `IndexError` at least once before. The recommended approach would be to avoid explicit iterator loops and use the `forEach`or `for-in`/`for-of-like` loop that takes care of the indexing, or `Stream`pipes. Still, one should consider whether he is not about to write [Clever Code](https://luzkan.github.io/smells/clever-code) and check if there is already a built-in function that will take care of the desired operation.

I would abstain from specifying all the loops as code smells. Loops have always been and probably are still going to be a fundamental part of programming. Modern languages offer very tidy approaches to loops and even things like a List Comprehension in [Haskell](https://wiki.haskell.org/List_comprehension) or [Python](https://docs.python.org/3/tutorial/datastructures.html). It is the indexation part might be the main problem. Of course, so are long loops or loops with side effects, but these are just a part of [Long Method](https://luzkan.github.io/smells/long-method) or [side effects](https://luzkan.github.io/smells/side-effects) code smells.

However, it is worth taking what is good from functional languages (such as `streams` or the immutability of the data) and implement those as widely as possible and conveniently, to increase the reliability of the application.

### Causation

It is impossible to easily overwrite the information given in the old books or video tutorials, which was pretty standard due to the lack of any other alternatives. People have learned that type of looping and may not even suspect that there are alternatives until they find them. Similarly, developers who come from older languages, which do not yet offer such facilities, can use explicit iteration habitually.

### Problems

#### **Readability**

In contrast to pipelines, loops don't provide declarative readability of what is precisely being processed.

### Example

#### Smelly

Explicitly Indexed Loop

```js
const examples = ["foo", "bar", "baz"]
for (let idx = 0; idx < examples.length; idx++) {
  console.log(examples[idx])
}
```

#### Solution

Handsome `forEach` Loop ("`For-In Loop`" / "`For-Of Loop`")

```js
const examples = ["foo", "bar", "baz"]
examples.forEach((example) => console.log(example))
```

#### Smelly

[Clever Code](https://luzkan.github.io/smells/clever-code), [Flag](https://luzkan.github.io/smells/flag-argument) and Explicit Iterator Loop

```js
const examples = ["foo", "bar", "baz"]
let bar_in_examples = false
for (let idx = 0; idx < examples.length; idx++) {
  if (examples[idx] == "bar") {
    bar_in_examples = true
  }
}
console.log(bar_in_examples) // true
```

#### Solution

Built-In function instead

```js
const examples = ["foo", "bar", "baz"]
console.log(examples.includes("foo")) // true
```

### Refactoring:

- Replace Loop with Pipeline
- Replace Loop with built-in