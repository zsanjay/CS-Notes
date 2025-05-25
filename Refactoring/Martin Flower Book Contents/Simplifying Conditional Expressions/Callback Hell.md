
Smell with a scent similar to the [Conditional Complexity](https://luzkan.github.io/smells/conditional-complexity), where tabs are intended deep, and the curly closing brackets can cascade like a Niagara waterfall.

The callback is a function that is passed into another function as an argument that is meant to be executed later. One of the most popular callbacks could be the `addEventListener` in JavaScript.

Alone in separation, they are not causing or indicating any problems. Instead, the long list of chained callbacks is something to watch out for. More professionally, this could be called a _Hierarchy of Callbacks_ but _(fortunately)_, it has already received a more exciting and recognizable name. There are many solutions to this problem: `Promises`, `async` functions, or splitting the significant function into separate methods.

### Problems:

#### Comprehensibility

Long and deep nested methods are challenging to read and maintain.

#### Inversion of Control Violation

One shouldn't give up control of how things are used.

### Example

#### Smelly

```js
const makeSandwich = () => {
  ...
  getBread(function(bread) {
    ...
    sliceBread(bread, function(slicedBread) {
      ...
      getJam(function(jam) {
        ...
        brushBread(slicedBread, jam, function(smearedBread) {
          ...
        });
      });
    });
  });
};
```

#### Solution

```js
const getBread = doNext => {
  ...
  doNext(bread);
};

const sliceBread = doNext => {
  ...
  doNext(breadSlice);
};

...

const makeSandwich = () => {
  return getBread()
    .then(bread => sliceBread(bread))
    .then(jam => getJam(beef))
    .then(slicedBread, jam => brushBread(slicedBread, jam));
};
```

### Refactoring:

- _Extract Methods_
- _Use Asynchronous Functions_
- _Use Promises_