https://rahulvijayvergiya.hashnode.dev/event-loop-and-concurrency-in-javascript

![image](https://github.com/user-attachments/assets/c81ca729-cdce-4de7-b623-3d83475b1546)


### What is the Event Loop?

Event loop is the heart of Node.js's asynchronous architecture. It is a mechanism that allows Node.js to perform non-blocking I/O operations, even though JavaScript is single-threaded. The event loop continuously checks the event queue to check what should be executed next, allowing Node.js to handle multiple tasks efficiently.

Here's how the event loop operates: 

1. **Call Stack**: JavaScript has a call stack that keeps track of function executions. When a function is called, it’s added to the stack. When it finishes executing, it’s removed from the stack.

2. **Task Queue (or Message queue)**: When an asynchronous operation completes (e.g., an API call, setTimeout), its callback is placed in the task queue.

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


---

### Nodejs Event Loop Phases

**How the Event Loop Works**

The event loop operates in cycles known as "ticks." Each tick represents a single pass through the phases of the event loop. During each tick, the event loop processes events in the phases.


**Event Loop Phases**
The Node.js event loop consists of six main phases:

1. Timers Phase
2. Pending Callbacks Phase
3. Idle, Prepare Phase
4. Poll Phase
5. Check Phase
6. Close Callbacks Phase

**1. Event loop Timers Phase**

**What happens**: This phase executes callbacks scheduled by setTimeout() and setInterval().

**Details**: Timers callbacks are executed once their scheduled time has passed. However, the actual execution time might be delayed if the previous phases take a long time to complete.

**2. Event loop Pending Callbacks Phase**

**What happens**: Executes I/O callbacks deferred to the next loop iteration.

**Details**: This phase handles callbacks for some system operations like TCP errors. These callbacks are not part of the timers phase because they are not scheduled using setTimeout or setInterval.

**3. Event loop Idle, Prepare Phase**

**What happens**: Internal use only.

**Details**: This phase is used internally by Node.js to prepare for the upcoming poll phase.

**4. Event loop Poll Phase**

**What happens**: Retrieves new I/O events; executes I/O related callbacks (almost all with the exception of close callbacks, timers, and setImmediate()); will block here when appropriate.

**Details**: This is the most important phase. Here, the event loop will pick up new events from the event queue and execute their callbacks. If there are no events to handle, it will block and wait for I/O events.

**5. Event loop Check Phase**

**What happens**: Executes setImmediate() callbacks.

**Details**: Callbacks scheduled with setImmediate() are executed here. This is similar to setTimeout() but it guarantees the callback will be executed immediately after the poll phase completes.

**6. Event loop Close Callbacks Phase**

**What happens**: Executes close callbacks (e.g., socket.on('close', ...)).

**Details**: This phase handles closing of all requests that need to be cleaned up. For example, closing of HTTP server or file descriptor.


---

### Understanding the Node.js Event Loop

Node.js is known for its non-blocking, asynchronous nature, which is made possible by its event-driven architecture.

**The Execution Context**
Before diving into the event loop, it's crucial to understand the execution context. An execution context holds information about the environment within which the current code is being executed. 
In Node.js, there are primarily two types of execution contexts:

**1. Global Execution Context**
: This is the default context in which code runs when a Node.js program starts. It creates the global object and sets up the environment.

**2. Function Execution Context**
: Each time a function is invoked, a new execution context is created for that function. This context contains the function's arguments, local variables, and a reference to the outer environment.

The execution context follows a lifecycle that includes creation and execution phases. During the creation phase, the lexical environment is set up, including variable and function declarations. In the execution phase, the code is executed, and variables are assigned values.

**The Call Stack**
The call stack is a data structure that keeps track of the execution context. It operates on a last-in, first-out (LIFO) principle. When a function is called, a new execution context is created and pushed onto the stack. When the function returns, its execution context is popped from the stack.

For example:

```
function foo() {
  console.log('foo');
  bar();
}

function bar() {
  console.log('bar');
}

foo();

```

The call stack operations for this code would look like this:

foo() is called, its context is pushed onto the stack.

Inside foo(), console.log('foo') is executed.

bar() is called, its context is pushed onto the stack.

Inside bar(), console.log('bar') is executed.

bar() finishes, its context is popped from the stack.

foo() finishes, its context is popped from the stack.


**The Event Loop**

The event loop is the core of Node.js's asynchronous capabilities. It allows Node.js to perform non-blocking I/O operations despite the fact that JavaScript is single-threaded. The event loop continuously checks the call stack and the callback queue, deciding what to execute next.

**Here's how the event loop operates with asynchronous code:**

1. Call Stack and Asynchronous Operations: When an asynchronous operation (like setTimeout, Promise, or I/O) is initiated, its callback is not executed immediately. Instead, the operation is handed off to the Node.js APIs, and the function's execution context is popped off the call stack.

2. Callback Queue (Macrotasks): Once the asynchronous operation is complete, its callback is placed in the callback queue (also known as the macrotask queue). This queue holds tasks like setTimeout callbacks, setInterval callbacks, I/O callbacks, and more.

3. Microtask Queue: In addition to the callback queue, there is a microtask queue, which holds microtasks like resolved Promise callbacks and process.nextTick callbacks. Microtasks have higher priority and are processed before the macrotasks.

4. Event Loop Execution:

The event loop first checks if the call stack is empty.

If the call stack is empty, it processes all the microtasks in the microtask queue.

After the microtask queue is empty, the event loop picks the first callback from the macrotask queue and pushes its execution context onto the call stack, executing it.

This process repeats, with the event loop continuously checking the call stack and processing microtasks and macrotasks as they become available.

**Conclusion** Node.js's ability to handle asynchronous operations efficiently is powered by the event loop, the call stack, and execution contexts. 
