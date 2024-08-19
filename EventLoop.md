https://rahulvijayvergiya.hashnode.dev/event-loop-and-concurrency-in-javascript

### What is the Event Loop?

Event loop is the core mechanism of javascript that allows it to perform non-blocking operations, even though it’s single-threaded. It continuously checks the call stack and the task queue (or message queue) to check what should be executed next.

Here's how the event loop operates: 

1. **Call Stack**: JavaScript has a call stack that keeps track of function executions. When a function is called, it’s added to the stack. When it finishes executing, it’s removed from the stack.

2. **Task Queue (or message queue)**: When an asynchronous operation completes (e.g., an API call, setTimeout), its callback is placed in the task queue.

3. **Event Loop**: The event loop continuously monitors the call stack and the task queue. If the call stack is empty, the event loop will push the first task from the task queue into the call stack, allowing it to be executed.


**Asynchronous Operations in JavaScript**

JavaScript uses different types of asynchronous operations, including:

**Timers (setTimeout, setInterval)**: These functions execute code after a specified delay.

**Promises**: Used for handling asynchronous operations, promises can be resolved or rejected and are managed by the microtask queue.

**Event Listeners**: Events such as clicks, key presses, and network requests trigger asynchronous callbacks.


Now lets understand the Macrotasks vs. Microtasks

### **Macrotasks vs. Microtasks in Event loop**

To understand the event loop better, we need to distinguish between macrotasks and microtasks.

**Macrotasks**

Macrotasks include operations like setTimeout, setInterval, I/O, and event callbacks. When these operations are complete, their callbacks are pushed into the task queue, waiting for the event loop to pick them up.

**Microtasks**

Microtasks, on the other hand, are primarily related to promise resolutions and MutationObserver. When a microtask is queued, it is processed after the currently executing script and before any macrotasks. This gives microtasks higher priority over macrotasks.


**How the Event Loop Handles Tasks**

Let’s break down the event loop with an example:

```
console.log('Start');

setTimeout(() => {
  console.log('Timeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('End');

```

output

```
Start
End
Promise
Timeout

```

**Here’s what happens:**

The synchronous code is executed first:

  console.log('Start') is executed and logged.

  setTimeout is encountered, and its callback is queued as a macrotask.

  Promise.resolve().then(...) is encountered, and its callback is queued as a microtask.

  console.log('End') is executed and logged.

The call stack is now empty, so the event loop checks the microtask queue:

The promise’s .then() callback is executed, logging Promise.
Only after the microtask queue is empty does the event loop move to the macrotask queue:
The setTimeout callback is executed, logging Timeout.
