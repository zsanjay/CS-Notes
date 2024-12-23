Promise callbacks are handled as a [microtask](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide) whereas [`setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout "setTimeout()") callbacks are handled as task queues.

```js

const promise = new Promise((resolve, reject) => {
  console.log("Promise callback");
  resolve();
}).then((result) => {
  console.log("Promise callback (.then)");
});

setTimeout(() => {
  console.log("event-loop cycle: Promise (fulfilled)", promise);
}, 0);

console.log("Promise (pending)", promise);

```

The code above will output:

```
Promise callback
Promise (pending) Promise {<pending>}
Promise callback (.then)
event-loop cycle: Promise (fulfilled) Promise {<fulfilled>}
```


#### Tasks vs. microtasks

A **task** is anything scheduled to be run by the standard mechanisms such as initially starting to execute a script, asynchronously dispatching an event, and so forth. Other than by using events, you can enqueue a task by using [`setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout "setTimeout()") or [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/setInterval "setInterval()").

The difference between the task queue and the microtask queue is simple but very important:

- When a new iteration of the event loop begins, the runtime executes the next task from the task queue. Further tasks and tasks added to the queue after the start of the iteration _will not run until the next iteration_.
- Whenever a task exits and the execution context stack is empty, all microtasks in the microtask queue are executed in turn. The difference is that execution of microtasks continues until the queue is empty—even if new ones are scheduled in the interim. In other words, microtasks can enqueue new microtasks and those new microtasks will execute before the next task begins to run, and before the end of the current event loop iteration.


## [Macrotasks and Microtasks](https://javascript.info/event-loop#macrotasks-and-microtasks)

Along with _macrotasks_, described in this chapter, there are _microtasks_, mentioned in the chapter [Microtasks](https://javascript.info/microtask-queue).

Microtasks come solely from our code. They are usually created by promises: an execution of `.then/catch/finally` handler becomes a microtask. Microtasks are used “under the cover” of `await` as well, as it’s another form of promise handling.

There’s also a special function `queueMicrotask(func)` that queues `func` for execution in the microtask queue.

**Immediately after every _macrotask_, the engine executes all tasks from _microtask_ queue, prior to running any other macrotasks or rendering or anything else.**

For instance, take a look:

```js

setTimeout(() => alert("timeout"));

Promise.resolve()

.then(() => alert("promise"));

alert("code");
```

What’s going to be the order here?

1. `code` shows first, because it’s a regular synchronous call.
2. `promise` shows second, because `.then` passes through the microtask queue, and runs after the current code.
3. `timeout` shows last, because it’s a macrotask.

All microtasks are completed before any other event handling or rendering or any other macrotask takes place.

That’s important, as it guarantees that the application environment is basically the same (no mouse coordinate changes, no new network data, etc) between microtasks.

If we’d like to execute a function asynchronously (after the current code), but before changes are rendered or new events handled, we can schedule it with `queueMicrotask`.

Here’s an example with “counting progress bar”, similar to the one shown previously, but `queueMicrotask` is used instead of `setTimeout`. You can see that it renders at the very end. Just like the synchronous code:

```js

<!doctype html>

<body>

<div id="progress"></div>

<script>

let i = 0;


function count() {


// do a piece of the heavy job (*)

do {

i++;

progress.innerHTML = i;

} while (i % 1e3 != 0);

  

if (i < 1e6) {

queueMicrotask(count);

}

}


count();

</script>

</body>
```

## [Summary](https://javascript.info/event-loop#summary)

A more detailed event loop algorithm (though still simplified compared to the [specification](https://html.spec.whatwg.org/multipage/webappapis.html#event-loop-processing-model)):

1. Dequeue and run the oldest task from the _macrotask_ queue (e.g. “script”).
2. Execute all _microtasks_:
    - While the microtask queue is not empty:
        - Dequeue and run the oldest microtask.
3. Render changes if any.
4. If the macrotask queue is empty, wait till a macrotask appears.
5. Go to step 1.

To schedule a new _macrotask_:

- Use zero delayed `setTimeout(f)`.

That may be used to split a big calculation-heavy task into pieces, for the browser to be able to react to user events and show progress between them.

Also, used in event handlers to schedule an action after the event is fully handled (bubbling done).

To schedule a new _microtask_

- Use `queueMicrotask(f)`.
- Also promise handlers go through the microtask queue.

There’s no UI or network event handling between microtasks: they run immediately one after another.

So one may want to `queueMicrotask` to execute a function asynchronously, but within the environment state.


[What will be the output of this code?](https://javascript.info/event-loop#what-will-be-the-output-of-this-code)

```
console.log(1);

setTimeout(() => console.log(2));

Promise.resolve().then(() => console.log(3));

Promise.resolve().then(() => setTimeout(() => console.log(4)));

Promise.resolve().then(() => console.log(5));

setTimeout(() => console.log(6));

console.log(7);
```

Solution:
 
```
The console output is: 1 7 3 5 2 6 4.
```