```js
Q1. What are Node.js modules?
Q2. What are the global objects of Node.js?
Q3. How to create a simple server in Node.js that returns Hello World?
Q4. Express vs NestJs
Q5. Routing
Q6. Difference between app.use vs app.get vs app.all vs router.get
Q7. app.js vs server.js
Q8. Thread Size and Increase

pending
Q. [Rest API and HTTP Methods]
Q. [Package.json vs Package-lock.json]
Q. [Ways to Improve performance]
Q. [Asynchronous and Synchronous]
```

**Q1. What are Node.js modules?**

We can refer to modules as small encapsulated units which can be reused and shared with anyone.
Easier to maintain and debug because they are separated pieces of code from each other.
Each module can also be stored in its own JavaScript file in its own folder. Then import that file when we need to use that module.

Node.js modules can be categorized into three types 

	Core modules — Inbuilt modules in the node.js
	User-created modules — Modules created by the user
	Third-party modules — Shared modules created and maintained by others

**Core modules**

	Basically, core modules are modules that are built into node.js these modules come automatically with the node.js installation.
	when you creating a node project these modules are available to use without any extra installations.
	before using core modules we need to import those modules into our project files using require keyword

	Syntax:
		const module = require('module_name');

Example:

	File System module
		It is used to perform operations on files.
		It can be accessed with require('fs').

	HTTP module
		It is used to create Http server and Http client. It can be accessed with require('http').

	OS module
		It is used to provide basic operating system related utility functions.
		It can be accessed with require('os').

	[For More Core Modules](https://www.dotnettricks.com/learn/nodejs/exploring-nodejs-core-modules)

[Node.js Local Module](https://www.tutorialsteacher.com/nodejs/nodejs-local-modules)

	Local modules are modules created locally in your Node.js application.
	These modules include different functionalities of your application in separate files and folders.
	For example, if you need to connect to MongoDB and fetch data then you can create a module for it,
	which can be reused in your application.

Export Module

**Log.js**
module.exports.log = function (msg) { 
    console.log(msg);
};

**app.js**

var msg = require('./Log.js');

msg.log('Hello World');

[Export Module](https://www.tutorialsteacher.com/nodejs/nodejs-module-exports)

---

``` js

** Q2. What are the global objects of Node.js?**
  
	Node.js Global Objects are the objects that are available in all modules.
	Global Objects are built-in objects that are part of the JavaScript and can be used directly
	in the application without importing any particular module.

These objects are modules, functions, strings and object itself as explained below.

	**1. global:**
	
		It is a global namespace. Defining a variable within this namespace makes it globally accessible.
		var myvar;
	
	**2. process:**
	
		It is an inbuilt global object that is an instance of EventEmitter used to get information on current process.
		It can also be accessed using require() explicitly.
	
	**3. console:**
	
		It is an inbuilt global object used to print to stdout and stderr.
	
		console.log("Hello World"); // Hello World
	
	**4. setTimeout(), clearTimeout(), setInterval(), clearInterval():**
	
		The built-in timer functions are globals
		
		function printHello() {
		   console.log( "Hello, World!");
		}
		
		// Now call above function after 2 seconds
		var timeoutObj = setTimeout(printHello, 2000);
	
	**5. __dirname:**
	
		It is a string. It specifies the name of the directory that currently contains the code.
	
		console.log(__dirname);
	
	**6. __filename:**
	
		It specifies the filename of the code being executed.
		This is the resolved absolute path of this code file.
		The value inside a module is the path to that module file.
	
		console.log(__filename);
```

---

**Q3. How to create a simple server in Node.js that returns Hello World?**

``` js
	/**
	 * Express.js
	 */
	const express = require('express');
	const app = express();
	
	app.get('/', function (req, res) {
	  res.send('Hello World!');
	});
	
	app.listen(3000, function () {
	  console.log('App listening on port 3000!');
	});
```
---

---
## <a id="expressNestJsIndex"> Express vs NestJs </a>
Q4. [Express & NestJs Comparison](https://medium.com/@karahanozen/express-js-vs-nest-js-2e39fc0ce22c)

```diff

Express and Nest are most known frameworks for creating back-end applications with Node.js

We can write code in nodejs but without framework we need to write lots of code. 
which is difficult to maintain, not error friendly when application grows.
also its difficult for unit testing.

to overcome this we used frameworks like express, nestjs.

- Express : 
	express handles request/response control, routing, serving static files, and middlewares for us. 


- Why NestJS Exist ?

the problem with express is architecture.
* Express is very minimalistic and straightforward, which provides flexibility to the users. 
* The flexibility is very beneficial for experienced users or complex scenarios, but the flexibility can cause
 an increase in errors and structural mistakes.
* In addition, applications nowadays require lots of extra logic like request validation, authorization,
documentation, testing, logging etc.
* So thinking the express only gives us a few features, people need to solve these requirements using other libraries or frameworks. 
That is why Nest.js exist.

- Nest.js

Nest is a framework for building efficient, scalable Node.js server-side applications.
Nest is built in top of common Node.js frameworks (Express, Fastify). 
It uses progressive JavaScript, is built with and fully supports TypeScript 
It has a simple design with 3 main components: controllers, modules and providers.

    + Controllers: 
        They handle the incoming requests and return responses to the client-side.

    + Providers: 
        These are the fundamental concepts of NestJS that you can treat as services, repositories, factories, helpers, etc.
        You can create and inject them into controllers or other providers as they are designed to abstract
	any complexity and logic.

    + Modules: 
        These are the classes. 
        A module is a reusable chunk of code that has a separate functionality. 
        The entire source code is organized and structured into modules. 
        Each application will have at least one root module which is the starting point.

   Diff between Experess & NestJs
	plz check the link	
```

---

**Q. Routing**

Routing refers to determining how an application responds to a client request to a particular endpoint,
which is a URI (or path) and a specific HTTP request method (GET, POST, and so on).

Each route can have one or more handler functions, which are executed when the route is matched.

Route definition takes the following structure:


	app.METHOD(PATH, HANDLER)
	Where:

	app is an instance of express.
	METHOD is an HTTP request method, in lowercase.
	PATH is a path on the server.
	HANDLER is the function executed when the route is matched.
 

These routing methods specify a callback function (sometimes called “handler functions”) called when the 
application receives a request to the specified route (endpoint) and HTTP method. 
In other words, the application “listens” for requests that match the specified route(s) and method(s), 
and when it detects a match, it calls the specified callback function.

The routing methods can have more than one callback function as arguments. With multiple callback functions, 
it is important to provide next as an argument to the callback function and then call next() within the body 
of the function to hand off control to the next callback.

The following examples illustrate defining simple routes.

Respond with Hello World! on the homepage:
	
	app.get('/', (req, res) => {
	  res.send('Hello World!')
	})
 ---

**Q. Difference between app.use vs app.get vs app.all vs router.get**

 app.use()
 
	 * It is generally used for introducing middlewares in your application and can handle all type of HTTP requests.
  
	 * Use app.use if you want to add some middleware (a handler for the HTTP request before it arrives to the routes
  	   you've set up in Express).
    
    	* We can also use app.use to enhance modularization in application (Express.Router written in a separate file)

	Ex. 1 : middleware
 
		//guard (create  middleware)
		let requiresLogin = function(req, res, next) {
		  if (! req.session.loggedIn) {
		    err = new Error("Not authorized");
		    next(err);
		  }
		  return next();
		};
	
		//protected route by using middleware - requiresLogin
		app.get('dashboard', requiresLogin, function (req, res) {
		  res.render('home');
		});

	Ex.2 : Enhance modularization by writting routing in seperate files

	In node.js, when we have multiple Express.Router written in a separate file to enhance modularization,
	we need to import each route file into the application server file and add lots of app.use() statements.  
	Now, if we add a new router we need to remember to add that too in the file where the application server is written.
	
		const express = require('express');
		const app = express();
		const port = 5000;
		
		// written in seperate files, and import here
		const userRouter = require('./routes/user.js')();
		const orderRouter = require('./routes/order.js')();
		
		// enhance modularization
		app.use(userRouter);
		app.use(orderRouter);
		
		app.listen(port, () => {
		  console.log(`My app listening at http://localhost:${port}`);
		});  	

link : https://www.codespeedy.com/create-separate-routes-file-in-node-express-js/

---

app.get() 
	 It is used only for handling GET HTTP requests.

	app.get('/', function (req, res) {
	  // ...
	});

Same thing for app.post, app.delete : It is used only for handling post/delete HTTP requests

---

app.use & app.all. 
	both can handle all kind of HTTP requests. 
	But there are some differences which recommend us to use app.use for middlewares and app.all for route handling.

	app.use() → It takes only one callback.
	app.all() → It can take multiple callbacks.


app.use() will only see whether url starts with specified path.

But, app.all() will match the complete path.

Here is an example to demonstrate this:

	app.use( "/product" , mymiddleware);
	// will match /product
	// will match /product/cool
	// will match /product/foo
	
	app.all( "/product" , handler);
	// will match /product
	// won't match /product/cool   <-- important
	// won't match /product/foo    <-- important
	
	app.all( "/product/*" , handler);
	// won't match /product        <-- Important
	// will match /product/cool
	// will match /product/foo

 ---
 app.get vs router.get
 
 	In Express.js, app.get() and router.get() are both used to handle HTTP GET requests to a specific route or path.
  	The main difference between the two is that 
   		app.get() is a method on the Express application instance, whereas
   		router.get() is a method on an instance of the Express Router.

		When you use app.get(), the route is added directly to the Express application, and the callback function will be 
  		executed when a GET request is made to the specified path.
  
  		On the other hand, when you use router.get(), the route is added to the router, which can then be mounted on
    		the Express application using the app.use() method.

In general, it is recommended to use router.get() when building modular applications as it allows to separate the different 
routes into different files and also to use router.use() to apply middleware to a specific set of routes.

---

**Q. app.js vs server.js**

Link : https://dev.to/superiqbal7/separating-app-and-server-in-express-why-it-matters-and-how-it-benefits-your-application-149d

app.js is responsible for defining the routes, middleware, and other application-level functionality. 

server.js, on the other hand, is responsible for creating the server, listening for incoming requests, and handling errors.

Thus we can keep our code organized and maintainable.

If we need to make changes to the application logic or routes, we can do so in the app.js file without worrying about the server setup. 
Similarly, if we need to make changes to the server configuration, we can do so in the server.js file without affecting the application logic.

app.js file:

	const express = require('express');
	const app = express();
	
	app.get('/', (req, res) => {
	  res.send('Hello World!');
	});
	
	module.exports = app;

server.js file:

	const app = require('./app');
	
	const PORT = process.env.PORT || 3000;
	
	app.listen(PORT, () => {
	  console.log(`Server listening on port ${PORT}`);
	});


In above code snippet, the app.js file is responsible for defining the Express application and its routes, 
while server.js creates a new HTTP server instance and passes it to the app module. 
This approach separates the server configuration from the application's functionality.

**Easier testing**

Separating app.js and server.js not only draws a clean separation of concerns but also significantly easy for mocking and testing the system by testing the API in-process, without performing network calls

**Improved scalability**

Finally, separating app.js and server.js can improve the application's scalability. By breaking up the code into smaller modules,
it becomes easier to add new features or modify existing ones without having to touch the entire application codebase.

---

**Q. Thread Size and Increase**

4 threads

By default, libuv uses a thread pool with 4 threads, but this number can be changed by setting the UV_THREADPOOL_SIZE environment variable. This means that you can increase or decrease the number of threads in the thread pool depending on the requirements of your application.

we are allowed to change the default value of 4 threads to anything up to 1024 threads. We achieve this by setting the UV_THREADPOOL_SIZE Node variable.
---
