**Q. What are Node.js modules?**

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

[Export Module](https://www.tutorialsteacher.com/nodejs/nodejs-module-exports)

---

-** Q. What are the global objects of Node.js?**
  
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

---

**- Q. How to create a simple server in Node.js that returns Hello World?**

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
Q. [Express & NestJs Comparison](https://medium.com/@karahanozen/express-js-vs-nest-js-2e39fc0ce22c)

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
Express is very minimalistic and straightforward, which provides flexibility to the users. 
The flexibility is very beneficial for experienced users or complex scenarios, but the flexibility can cause an increase in errors and structural mistakes.
In addition, applications nowadays require lots of extra logic like request validation, authorization, documentation, testing, logging etc.
So thinking the express only gives us a few features, people need to solve these requirements using other libraries or frameworks. 
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
        You can create and inject them into controllers or other providers as they are designed to abstract any complexity and logic.

    + Modules: 
        These are the classes. 
        A module is a reusable chunk of code that has a separate functionality. 
        The entire source code is organized and structured into modules. 
        Each application will have at least one root module which is the starting point.

   Diff between Experess & NestJs
	plz check the link	
```
---

