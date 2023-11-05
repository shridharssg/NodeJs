**Q6. Error Handling in nodeJS**
https://blog.appsignal.com/2022/11/16/nodejs-error-handling-tips-and-tricks.html

https://medium.com/backenders-club/error-handling-in-node-js-ef5cbfa59992

Error handling a mandatory step in application development.

**Common Types of Errors**

Programming Errors : These errors generally occur in development (when writing our code).
Examples of such errors are syntax errors, logic errors, poorly written asynchronous code

Most syntax and type errors can be minimized by using Typescript linting Tslint, eslint, jshint, and jslint.

**Operational Errors**

These are the kind of errors that should be handled in our code. They represent runtime problems like 
invalid inputs, database connection failure, unavailability of resources

**Handling Errors in Node.js**

Errors in Node.js are handled as exceptions.
Once an exception is unhandled, Node.js shuts down your program immediately.
But with error handling, you have the option to decide what happens and handle errors gracefully.
Node.js provides multiple mechanisms to handle errors.

1. Try Catch

2. Error-First Callback Error Handling
    if the callback operation completes and an exception is raised,
    the callback's first argument is an Error object. Otherwise, the first argument is null, indicating no error.
```
const fs = require("fs");
fs.open("somefile.txt", "r+", (err, fd) => {
  // if error, handle error
  if (err) {
    return console.error(err.message);
  }
  // operation successful, you can make use of fd
  console.log(fd);
});
```

3. Promise Error Handling
    Using async/await with the try...catch construct
    Using the .catch() method on promises

```
function doSomeThingAsync() {
  return new Promise((resolve, reject) =>
    setTimeout(() => reject(new Error("some error")), 5000)
  );
}
async function run() {
  try {
    await doSomeThingAsync();
  } catch (err) {
    console.log("ERROR: ", err);
  }
}
run();
```

```
function doSomeThingAsync() {
  return new Promise((resolve, reject) =>
    setTimeout(() => reject(new Error("some error")), 5000)
  );
}
doSomeThingAsync()
  .then((resolvedValue) => {
    // resolved the do something
  })
  .catch((err) => {
    // an exception is raised
    console.error("ERROR: ", err.message);
  });
```

**4. Event Emitters**

EventEmitter is another style of error handling used commonly in an event-based API. 
This is useful in continuous or long-running asynchronous operations where a series of errors can happen. 

Here's an example: file read operation in stream
