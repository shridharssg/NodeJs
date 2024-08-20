
![image](https://github.com/user-attachments/assets/c81ca729-cdce-4de7-b623-3d83475b1546)
![image](https://github.com/user-attachments/assets/9ad1c4e0-3dd0-4481-9c73-6639a7c5fd5a)


### What is the Event Loop?

Event loop is the heart of Node.js's asynchronous architecture. It is a mechanism that allows Node.js to perform non-blocking I/O operations, even though JavaScript is single-threaded. The event loop continuously checks the event queue to check what should be executed next, allowing Node.js to handle multiple tasks efficiently.

Javascript is a single-threaded and non-blocking language, single-threaded means it can only do one task at a time, and non-blocking means, if there are tasks that are taking too long to complete then javascript don't block the execution of the other tasks.
Well If you have used javascript it doesn't block the execution of the other tasks while performing an asynchronous task so how it's doing that, well it uses different tools like call stack, memory heap, web APIs, Callback queue, and event loop

**Here's how the event loop operates:**

Browsers have something called an engine that runs javascript code for you, some popular ones are v8 in chrome and Webkit in Safari. A Browser engine that runs Javascript consists of two things a heap and a call stack.

**Heap** is the area where the memory of objects, arrays, functions, and other variables of your javascript program is stored dynamically and randomly.

**Call Stack** : 
  Call Stack is where all functions or tasks are stacked and executed in the manner of LIFO(last-in-first-out) order, when the program is running line by line and it encountered a function then the function is pushed to call stack and when the function is executed then it is popped out of call stack. but say when the program is running and encounters an asynchronous task, it is pushed to the call stack and the call stack pushes it to web APIs because the call stack only deals with the synchronous tasks so that it won't get blocked by asynchronous tasks.

**Web APIs** 
  Web APIs are part of the browser that provides us with APIs such as HTTP, AJAX, Geolocation, DOM events, fetch, setTimeout, etc, which we can call in our JavaScript code and the execution is handled by browsers itself. Now when browser web APIs have some task sent by call stack it performs the task but doesn't send back to the call stack directly but instead gives it a callback and pushes to Callback Queue (Macrotask) or Microtask Queue depending on the callback.

**Callback Queue and Microtask Queue**
  are the area where callbacks sent by web APIs are stored and are getting ready to be sent to the call stack to get executed. Callback Queue and Microtask Queue store the callbacks in a FIFO(first-in-first-out) fashion but who decides when to send them to the call stack that's where the event loop comes.
  
  Note : Callback Queue : callbacks related to : setTimeout callbacks, setInterval callbacks, I/O callbacks
  Note : Microtask Queue : callbacks related to : Promise callbacks and process.nextTick callbacks

  
**Event Loop** 
The event loop continuously monitors the call stack and Callback Queue and Microtask Queue.
Its check if the call stack is empty or not and if there are any callback functions inside the callback queue or microtask queue and if there is then one by one event loop sends to the call stack to get executed. All the callback functions coming through Promises will go inside the Microtask Queue and callback functions coming from the setTimeout(), etc goes inside the callback queue. Microtask queue has a higher priority than callback queue so event loop priorities sending from Microtask queue first then move to the Callback queue.

---

**Asynchronous Operations in JavaScript**

JavaScript uses different types of asynchronous operations, including:

**Timers (setTimeout, setInterval)**: These functions execute code after a specified delay.

**Promises**: Used for handling asynchronous operations, promises can be resolved or rejected and are managed by the microtask queue.

**Event Listeners**: Events such as clicks, key presses, and network requests trigger asynchronous callbacks.

---

### **Macrotasks vs. Microtasks in Event loop**

To understand the event loop better, we need to distinguish between macrotasks and microtasks.

**Macrotasks**

Macrotasks include operations like setTimeout, setInterval, I/O, and event callbacks. When these operations are complete, their callbacks are pushed into the task queue, waiting for the event loop to pick them up.

**Microtasks**

Microtasks, on the other hand, are primarily related to promise resolutions and MutationObserver. When a microtask is queued, it is processed after the currently executing script and before any macrotasks. This gives microtasks higher priority over macrotasks.

---

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

### Understanding Execution Context Before Node.js Event Loop

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


**Conclusion** 

The event loop is the core of Node.js's asynchronous capabilities. It allows Node.js to perform non-blocking I/O operations despite the fact that JavaScript is single-threaded. The event loop continuously checks the call stack and the callback queue, deciding what to execute next.

Node.js's ability to handle asynchronous operations efficiently is powered by the event loop, the call stack, and execution contexts. 
