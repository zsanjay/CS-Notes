
### What `setTimeout(fn, 0)` Actually Does

It tells the browser to:

> “Execute `fn` **after putting it in the event queue**, and only when the **call stack is empty** and the current execution context is done.”

Even with a `0` millisecond delay, the function will not execute **immediately**. It will be **deferred** until after the current synchronous code is complete.


```js
console.log("Start");

setTimeout(() => {
  console.log("Inside setTimeout");
}, 0);

console.log("End");
```


```shell
Start
End
Inside setTimeout
```

Even though the delay is `0`, the callback runs **after** the synchronous code (`console.log("End")`) is done.

