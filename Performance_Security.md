### To prevent Node.js from continuously hitting an API or malacious attack, you can implement the following strategies:
### Or How to increase application performance

To prevent Node.js from continuously hitting an API, you can implement the following strategies:
 
1. *Rate Limiting*: 
               Set a limit on the number of requests made to the API within a certain time frame. Use libraries like `rate-limiter-flexible` or `limiter` to achieve this. **or**
               Slows down responses rather than blocking them outright. Use to slow repeated requests to public APIs and/or endpoints such as password reset. Use libraries like 'express-slow-down' to achieve this.
 
2. **Caching**: Cache API responses in memory or a database to reduce the number of requests made. Use libraries like `node-cache` or `redis`.
 
3. **Debouncing**: Group multiple requests into a single request using debouncing techniques. Use libraries like `lodash.debounce` or `debounce`.
 
4. **Throttling**: Limit the number of requests made within a certain time frame. Use libraries like `throttle-debounce` or `p-throttle`.
 
5. ***Exponential Backoff***: Gradually increase the delay between requests after each failure. Use libraries like `backoff` or `retry-axios`.
 
6. ***Circuit Breaker Pattern***: Temporarily stop making requests when the API is down or responding with errors. Use libraries like `opossum` or `circuit-breaker-js`.
 
7. ***API Keys or Tokens***: Use API keys or tokens to authenticate and limit requests. Check with the API provider for their usage guidelines.
 
8. ***Request Queueing***: Queue requests and process them in batches to reduce the load on the API.
 
9. ***Monitor API Usage***: Keep an eye on API usage metrics to detect and prevent excessive requests.
 
10. ***Implement API Provider Guidelines***: Follow the guidelines and limits set by the API provider to avoid abuse and termination of service.
 
By implementing these strategies, you can prevent Node.js from continuously hitting an API and ensure a more reliable and efficient application.
 

---

## Rate limiting
https://blog.appsignal.com/2024/04/03/how-to-implement-rate-limiting-in-express-for-nodejs.html

Rate limiting is a fundamental mechanism for controlling the number of requests a client can make to a server in a given time frame

Rate limiting is a strategy for limiting network traffic by placing a cap on how often an actor can call the same API in a given time frame. That's essential for controlling the volume of requests a client can make to a server in a certain amount of time.

To better understand how this mechanism works, consider a scenario where you have a Node.js backend that exposes some public endpoints. Suppose a malicious user targets your server and writes a simple script to overload it with automated requests. The performance of your server will downgrade as a result, hindering the experience of all other users. In an extreme situation, your server may even go offline. By limiting the number of requests the same IP can make in a given time frame, you can avoid all that!

Specifically, there are two approaches to rate limiting:

**Blocking incoming requests:** When a client exceeds the defined limits, deny its additional requests.

**Slowing down requests:** Introduce a delay for requests beyond the limits, making the caller wait longer and longer for a response.

controlling the rate at which requests are processed helps enforce usage limits, prevents server overloads, and safeguards against malicious attacks.

### Blocking incoming requests

```
const express = require("express");

const port = 3000;

// initialize an Express server
const app = express();

// define a sample endpoint
app.get("/hello-world", (req, res) => {
  res.send("Hello, World!");
});

// start the server
app.listen(port, () => {
  console.log(`Server listening at http://hostname:${port}`);
});

```

 How to integrate rate limiting behavior into a Node.js application using the following libraries:

**express-rate-limit:** To block requests that exceed specified limits.

**express-slow-down:** To slow down similar requests coming from the same actor.

```

const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 60 * 1000, // 1 minute
  limit: 10, // maximum number of requests allowed in the windowMs
  message: 'Too many requests, please try again later.',
});

// option 1 : Register the limiter middleware to all endpoints with:

   app.use(limiter);

// option 2 : If you instead want to rate limit APIs under a specific path, use the limiter middleware as below:

   app.use("/public", limiter);

// option 3 : To protect only a certain endpoint, pass limiter as a parameter in the endpoint definition:

   app.get("/hello-world", limiter, (req, res) => {
     // ...
   });

```

Testing Rate Limiting

Open the postman and test it.
On the eleventh API call within a 1-minute time frame, the request will fail, with the error: “Too many requests, please try again later.”


### Slowing Down Requests With express-slow-down

```
const { rateLimit } = require("express-slow-down");

const limiter = slowDown({
  windowMs: 15 * 60 * 1000, // 5 minutes
  delayAfter: 10, // allow 10 requests per `windowMs` (5 minutes) without slowing them down
  delayMs: (hits) => hits * 200, // add 200 ms of delay to every request after the 10th
  maxDelayMs: 5000, // max global delay of 5 seconds
});

app.use(limiter);       // or other option which mentioned in above example

```

---

## Debounce and Throttling
https://medium.com/@bs903944/debounce-and-throttling-what-they-are-and-when-to-use-them-eadd272fe0be

If you are a web developer, you might have encountered situations where you need to optimize the performance of your code that runs repeatedly within a short period of time. For example, you might have a search bar that fetches suggestions from the backend as the user types, or a resize event handler that adjusts the layout of your page. In these cases, you don’t want to execute your code too often, as it might cause unnecessary network requests, a laggy user interface, or high CPU usage.

To solve this problem, you can use two techniques called debouncing and throttling. These techniques allow you to control the rate at which your code is executed, and reduce the number of times it is called.

### What is Debouncing?
Debouncing is a technique that delays the execution of a function until the user stops performing a certain action for a specified amount of time. For example, if you have a search bar that fetches suggestions from the backend as the user types, you can debounce the function that makes the API call, so that it only runs after the user stops typing for a few seconds. This way, you can avoid making too many API calls that might overload your server or return irrelevant results.

To implement debouncing in JavaScript, you can use a timer variable to track the delay period. You can use the setTimeout function to set a timer that will execute your function after the delay period. You can also use the clearTimeout function to cancel the timer if the user performs the action again before the delay period ends. This way, you can ensure that your function only runs once after the user stops performing the action.

Here is an example of how to implement debouncing in JavaScript:

```
  // A function that makes an API call with the search query
  function searchHandler(query) {
      // Make an API call with search query
      getSearchResults(query);
  }
  // A debounce function that takes a function and a delay as parameters
  function debounce(func, delay) {
      // A timer variable to track the delay period
      let timer;
      // Return a function that takes arguments
      return function(…args) {
          // Clear the previous timer if any
          clearTimeout(timer);
          // Set a new timer that will execute the function after the delay period
          timer = setTimeout(() => {
              // Apply the function with arguments
              func.apply(this, args);
          }, delay);
      };
  }
  // A debounced version of the search handler with 500ms delay
  const debouncedSearchHandler = debounce(searchHandler, 500);
  // Add an event listener to the search bar input
  searchBar.addEventListener("input", (event) => {
      // Get the value of the input
      const query = event.target.value;
      // Call the debounced search handler with the query
      debouncedSearchHandler(query);
  });

```
In this example, we have a searchHandler function that makes an API call with the search query. We also have a debounce function that takes a function and a delay as parameters, and returns a debounced version of that function. We use this debounce function to create a debouncedSearchHandler function with 500ms delay. We then add an event listener to the search bar input, and call the debouncedSearchHandler function with the input value. This way, we can ensure that we only make one API call after the user stops typing for 500ms.

## What is Throttling?

Throttling is a technique that limits the execution of a function to once in every specified time interval. For example, if you have a resize event handler that adjusts the layout of your page, you can throttle the function that updates the layout, so that it only runs once every 100ms. This way, you can avoid running your code too frequently, which might cause janky user interface or high CPU usage.

To implement throttling in JavaScript, you can use a flag variable to track whether the function is already running or not. You can use the setTimeout function to set a timer that will reset the flag after the time interval. You can also use an if statement to check whether the flag is true or not before executing your function. This way, you can ensure that your function only runs once in every time interval.

Here is an example of how to implement throttling in JavaScript:

 ```
    // A function that updates the layout of the page
    function updateLayout() {
        // Update layout logic
    }
    // A throttle function that takes a function and an interval as parameters
    function throttle(func, interval) {
        // A flag variable to track whether the function is running or not
        let isRunning = false;
        // Return a function that takes arguments
        return function(…args) {
            // If the function is not running
            if (!isRunning) {
                // Set the flag to true
                isRunning = true;
                // Apply the function with arguments
                func.apply(this, args);
                // Set a timer that will reset the flag after the interval
                setTimeout(() => {
                    // Set the flag to false
                    isRunning = false;
                }, interval);
            }
        };
    }
    // A throttled version of the update layout function with 100ms interval
    const throttledUpdateLayout = throttle(updateLayout, 100);
    // Add an event listener to the window resize event
    window.addEventListener("resize", () => {
        // Call the throttled update layout function
        throttledUpdateLayout();
    });
 ```

In this example, we have an updateLayout function that updates the layout of the page. We also have a throttle function that takes a function and an interval as parameters, and returns a throttled version of that function. We use this throttle function to create a throttledUpdateLayout function with 100ms interval. We then add an event listener to the window resize event, and call the throttledUpdateLayout function. This way, we can ensure that we only update the layout once every 100ms.

## What is the Difference Between Debouncing and Throttling?

The main difference between debouncing and throttling is that debouncing executes the function only after some cooling period, while throttling executes the function at a regular interval. Debouncing and throttling are both useful techniques to improve the performance of your code, but they have different use cases and effects.

Debouncing is useful when you want to delay the execution of your code until the user stops performing a certain action. For example, you can use debouncing for autocomplete, where you want to wait for the user to stop typing before fetching suggestions from the backend. Debouncing can reduce the number of times your code is executed, but it can also introduce some latency in your user interface.

Throttling is useful when you want to limit the execution of your code to a certain frequency. For example, you can use throttling for resize, where you want to update the layout of your page at a fixed rate. Throttling can improve the responsiveness of your user interface, but it can also cause some loss of information or accuracy in your code.

---

## How to remove unused libraries or modules in a Node.js application, 

follow these steps:
 
1. *Identify unused dependencies*:
    - Use tools like `depcheck` or `unused-deps` to scan your project and identify unused dependencies.
    - Run `depcheck` or `unused-deps` in your terminal/command prompt to get a list of unused dependencies.
2. *Remove unused dependencies from `package.json`*:
    - Open your `package.json` file and remove the unused dependencies from the `dependencies` or `devDependencies` section.
3. *Remove unused modules from your code*:
    - Search for `require` or `import` statements in your code that reference the unused modules.
    - Remove or comment out these statements to prevent the modules from being loaded.
4. *Run `npm prune` or `yarn prune`*:
    - Run `npm prune` or `yarn prune` to remove unused dependencies from your `node_modules` directory.
5. *Verify and test*:
    - Verify that your application still works as expected after removing unused dependencies.
    - Run tests to ensure that removing unused dependencies didn't break any functionality.
 
Some popular tools to help you identify and remove unused dependencies include:
 
- `depcheck`: A command-line tool to check for unused dependencies.
- `unused-deps`: A tool to identify unused dependencies in your project.
- `npm-unused`: A tool to identify and remove unused dependencies.
- `yarn unused`: A command to identify unused dependencies in your project.

```
npm install depcheck

for run 
depcheck /path/to/my/project

output :

Unused dependencies
* underscore
Unused devDependencies
* jasmine
Missing dependencies
* lodash

```
The **dependency underscore** is declared in the package.json file, but not used by any code.
The **devDependency jasmine** is declared in the package.json file, but not used by any code.
The **dependency lodash** is used somewhere in the code, but not declared in the package.json file.

Please note that, if a subfolder has a package.json file, it is considered another project and should be checked with another depcheck command.
Remember to always verify and test your application after removing unused dependencies to ensure that everything works as expected.

---

### ACL - Access Control List : role-based access control

ACL stands for Access Control List, which is a mechanism used to manage permissions and access to resources in a system. In Node.js, ACL can be implemented using various libraries and frameworks, such as:
 
1. acl (npm package): A popular library for managing ACL in Node.js applications.
2. access-control (npm package): Another library for implementing ACL in Node.js.
3. Node-ACL (npm package): A simple ACL library for Node.js.
4. CASL (npm package): A more advanced library for managing permissions and ACL in Node.js applications.
 
These libraries provide features like role-based access control, permission management, and more.

To implement API-specific permissions, you can modify the `allows` property to include specific API endpoints and HTTP methods. Here's an updated example:

In below example:
- We define three roles: `admin`, `moderator`, and `user`.
- The `admin` role has full access to both `/api/users` and `/api/posts`.
- The `moderator` role can `GET`, `POST`, and `PUT` on `/api/posts`, but not `DELETE`.
- The `user` role can only `GET` and `POST` on `/api/posts`.
- We specify the permissions for each role using the `allows` property.
- We then check if a specific role has permission to perform an action on a resource using the `isAllowed` method.
- By separating the permission-checking logic into its own middleware function (`checkPermissions`), you can easily reuse it across multiple routes or even in other parts of your application.

```
const express = require('express');
const acl = require('acl');
 
const app = express();
 
// Initialize ACL
const aclInstance = new acl(new acl.memoryBackend());
 
// Define roles and permissions
aclInstance.allow([
  {
    roles: 'admin',
    allows: [
      { resources: '/api/users', permissions: ['GET', 'POST', 'PUT', 'DELETE'] },
      { resources: '/api/posts', permissions: ['GET', 'POST', 'PUT', 'DELETE'] }
    ]
  },
  {
    roles: 'moderator',
    allows: [
      { resources: '/api/posts', permissions: ['GET', 'POST', 'PUT'] }
    ]
  },
  {
    roles: 'user',
    allows: [
      { resources: '/api/posts', permissions: ['GET', 'POST'] }
    ]
  }
]);
 
// Separate middleware function to check permissions

const checkPermissions = (req, res, next) => {
  const role = req.user.role; // Assuming you have a way to get the user's role
  const resource = req.path;
  const method = req.method;
 
  aclInstance.isAllowed(role, resource, method, (err, allowed) => {
    if (err) {
      return res.status(500).send({ message: 'Internal Server Error' });
    }
 
    if (!allowed) {
      return res.status(403).send({ message: 'Access denied' });
    }
 
    next();
  });
};
 
    // Use the middleware function in the app
app.use(checkPermissions);
 
   // API routes
app.post('/api/posts', (req, res) => {
  // Handle POST request
});
 
app.listen(3000, () => {
  console.log('Server listening on port 3000');
});
```
