
### How to resolve unhandled exceptions in node?

In Node.js, unhandled exceptions can be resolved using the process.on(‘uncaughtException’) event. By attaching a listener to this event, you can catch unhandled exceptions and prevent Node.js from terminating.

```
process.on('uncaughtException', (err) => {
  console.error('An unhandled exception occurred:', err);
  // Perform cleanup, logging, or any necessary action
  // Avoid attempting to continue with the application as it may be in an inconsistent state
  // Gracefully shut down the application
  **process.exit(1);** // Exit the process with a failure code (1)
});

// Example of an unhandled exception (for demonstration purposes)
// This code will throw an unhandled exception
setTimeout(() => {
  throw new Error('Intentional unhandled exception');
}, 100);
```


// Other application logic
// ...
When an unhandled exception occurs, the provided callback function is executed, allowing you to log the error, perform any necessary cleanup, and gracefully shut down the application using process.exit(1).

---

### Error Handling in nodeJS**


Error handling a mandatory step in application development.

**Common Types of Errors**

   **Programming Errors :** 
   
        These errors generally occur in development (when writing our code).
        Examples of such errors are syntax errors, logic errors, poorly written asynchronous code
        Most syntax and type errors can be minimized by using Typescript linting Tslint, eslint, jshint, and jslint.

 **Operational Errors**

        These are the kind of errors that should be handled in our code. 
        They represent runtime problems like invalid inputs, database connection failure, unavailability of resources

---

**Handling Errors in Node.js**

Errors in Node.js are handled as exceptions.
Once an exception is unhandled, Node.js shuts down your program immediately.
But with error handling, you have the option to decide what happens and handle errors gracefully.
Node.js provides multiple mechanisms to handle errors.

In Node.js, errors can be handled using a variety of techniques, including try…catch blocks, error-first callbacks, and the use of error event emitters.

1. Try Catch
     ```
     try {
        // Code that may throw an error
        const result = someFunction();
        console.log(result);
      } catch (error) {
        console.error('An error occurred:', error);
      }
     
     ```

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

const myEmitter = new MyEmitter();

myEmitter.on('error', (err) => {
  console.error('An error occurred:', err);
});

myEmitter.emit('error', new Error('Oops! Something went wrong.'));

---


**Understanding Normal Middleware and Error Handling Middleware in Express.Js**


In Express, middleware plays a crucial role in processing incoming requests and preparing the appropriate responses.

Middleware functions are designed to handle specific tasks and are executed sequentially in the order they are registered. 
However, it’s essential to understand the distinction between normal middleware and error-handling middleware, 
as they serve different purposes and exhibit different behaviours.

**Normal Middleware:**

Normal middleware functions in Express typically have three parameters: req, res, and next. 
They handle regular requests, perform specific operations, and pass control to the next middleware or route handler.

```
const normalMiddleware = (req, res, next) => {
  // Perform some operations
  console.log('Executing normal middleware');
  next(); // Pass control to the next middleware or route handler
};

```
In the above code, 
the normalMiddleware function performs some operations and then calls next() to pass control 
to the next middleware or route handler in the chain. 
Normal middleware functions are responsible for tasks like logging, request parsing, authentication, and authorization.

---

**Error Handling Middleware:**

Error-handling middleware functions in Express have four parameters: err, req, res, and next. 
They handle errors that occur during the request-response cycle. When an error occurs, Express looks for 
the next error-handling middleware in the middleware stack, skipping any normal middleware along the way. 

Here's an example of an error-handling middleware:

```
const errorHandlerMiddleware = (err, req, res, next) => {
  // Handle the error
  console.log('Executing error handling middleware');
  res.status(500).json({ error: 'Internal Server Error' });
};

```
In the above code snippet, the errorHandlerMiddleware function receives the err parameter, 
indicating that it is an error-handling middleware.It handles the error and sends an appropriate response. 
Error handling middleware functions are responsible for catching and processing errors, providing meaningful error messages,
and taking necessary actions based on the error type

---

**Behaviour Differences:**

To observe the behaviour differences between normal middleware and error-handling middleware, 
let’s consider the following Express application setup:

```
const express = require('express');
const app = express();

app.use(normalMiddleware);
app.use(errorHandlerMiddlewareBeforeRoute);
app.get('/', (req, res) => {
  try {
    // Some route handling code
  } catch (err) {
    next(err); // Pass the error to the next middleware
  }
});
app.use(normalMiddlewareAfterRoute);
app.use(errorHandlerMiddlewareAfterRoute);
```

In this setup, the normalMiddleware is registered before the route handler, 
and the errorHandlerMiddlewareBeforeRoute is registered after it. 

Scenario 1: Normal Request Handling

When a request arrives at the “/” route, Express starts processing the middleware chain. 
It encounters the, normalMiddleware executes it, and passes control to the route handler. 
However, since it's a regular request without any errors, the errorHandlerMiddlewareBeforeRoute is not invoked.
The response is sent directly to the client without any error handling.

Scenario 2: Error Occurrence
In this scenario, let’s assume that an error occurs within the route handler code:

```
app.get('/', (req, res) => {
  try {
    // Some route handling code
  } catch (err) {
    next(err); // Pass the error to the next middleware
  }
});
```
Now, when a request is made to the “/” route and an error is thrown within the route handler, 
the error is caught in the catch block. By calling next(err), the error is passed to the next middleware in the chain.
Since the errorHandlerMiddlewareAfterRoute is registered after the route handler, it will be invoked to handle the 
error ignoring the normal middleware normalMiddlewareAfterRoute. 

The errorHandlerMiddlewareAfterRoute receives the error object (err) as its first parameter and can handle the error appropriately.
It can send an error response to the client or perform any other necessary actions based on the error.

---

To handle errors consistently and efficiently, it’s recommended to use dedicated error-handling middleware, placed after all other routes and middleware. This ensures that any unhandled errors are caught and handled centrally, preventing crashes and providing meaningful responses to clients.

By leveraging the appropriate middleware for each task and considering the order in which they are registered, you can effectively control the flow of requests and responses and handle errors gracefully.
