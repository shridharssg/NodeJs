# nodejs

Node JS Platform
Node JS Platform uses “Single Threaded Event Loop” architecture to handle multiple concurrent clients. 
Then how it really handles concurrent client requests without using multiple threads. What is Event Loop model? We will discuss these concepts one by one. 
Before discussing “Single Threaded Event Loop” architecture, first we will go through famous “Multi-Threaded Request-Response” architecture.


**The 6 phases of the Node.js event**
Let’s look at a code snippet first:


setImmediate(()=> console.log('setImmediate'));
fs.readFile('/etc/passwd',(err, data)=>{
  console.log('reading file');
}); 
console.log('start');
process.nextTick(()=> console.log('nextTick'));
setTimeout(()=>console.log('setTimeout 1'), 0);
setTimeout(()=>console.log('setTimeout 2'), 3);
let counter = 0;
const timeout = setInterval(() => {
    console.log('setInterval');
    if (counter >= 3) {
        console.log('exiting setInterval');
        clearInterval(timeout);
    }
    counter++;
}, 0);
new Promise((resolve, reject)=> {
  console.log('start promise 1');
  resolve('Promise 1');
}).then(data=> {
  console.log(data);
})
console.log('end');


**What would be the output of this program?**

In order to answer the question, we’ll need to understand 2 things:

1. The different phases in the event loop.
2. The tasks each phase will handle.

The 6 phases
When Node.js finished executing the index.js in the main thread, the Node.js event loop starts to execute the callbacks registered during the main thread’s runtime.

![image](https://github.com/shridharssg/nodejs/assets/139750756/9bff4dab-8d5a-43a1-aa30-908803ca2e47)


**Callback queue**
There’s a callback queue in each phase that stores callbacks to be executed in that phase. It’s very similar to the Task queue provided by a browser.

**Timers**
This is the first phase in the event loop. It finds expired timers in every iteration (also known as Tick) and executes the timers callbacks created by setTimout and setInterval.

For example:

setTimeout(() => console.log('Timeout 1'), 0);
setTimeout(() => console.log('Timeout 2'), 10);

Timeout 1 will be printed in the first iteration because the first timer expires after 0ms. 
However, Timeout 2 will be printed in another iteration (not necessarily in the second iteration) because the second timer expires after 10ms.

**Pending callbacks**
It handles I/O callbacks deferred to the next iteration, such as handling TCP socket connection error.

**Idle, prepare**
It’s only used internally.

**Poll**
The Poll phase calculates the blocking time in every iteration to handle I/O callbacks. In this phase, the epoll_wait() system call is invoked (in Linux).

**Check**
This phase handles the callbacks scheduled by setImmediate(), and the callbacks will be executed once the Poll phase becomes idle.

**Close Callback**
This phase handles callbacks if a socket or handle is closed suddenly and the ‘close’ event will be emitted.

**The microtask queue**
The microtask queue stores microtasks (callbacks) created by:

process.nextTick()
then() and catch() that handle resolved and rejected promises
Microtasks are executed after the main thread and each phase of the event loop. Microtasks created by process.nextTick() are executed before those created by then() and catch().

![image](https://github.com/shridharssg/nodejs/assets/139750756/5dfe59cb-406e-4c66-8a3e-44091a5c9cf0)
