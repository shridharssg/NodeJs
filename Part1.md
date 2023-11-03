# <a id="nodeJSIndex"> NodeJs </a>

Q. [Promises and Types](#promisesIndex)

Q. [Async Await](#asyncAwait)

Q. [Json Web Token](#jsonWebTokenIndex)

Q. [Validate API with JWT](#validateAPI)

Q. [Event Emitter and Events](#eventIndex)

Q. [Middleware and Types](#middlewareIndex)

Q. [Microservices](#microIndex)

Q. [Secure Rest API](#securityIndex)

Q. [Architecture And Event Loop Phases](#architectureIndex)

Q. [File System](#fsIndex)

Q. [Data Stream](#dataIndex)

Q. [Clutser](#cluster)

---

## <a id="promisesIndex"> Promises </a>
Q. [Promise](https://dotnettutorials.net/lesson/javascript-promise/)
Q. [Promise Chaining](https://dotnettutorials.net/lesson/promise-chaining-in-javascript/)
Q. [Promise Types](https://dotnettutorials.net/lesson/javascript-promise-race-vs-promise-all/)

---

## <a id="asyncAwait"> Async Await </a>
Q. [Async Await](https://dotnettutorials.net/lesson/javascript-async-await/)

---

## <a id="jsonWebTokenIndex"> Json Web Token </a>
Q. [JSON Web Token](https://www.bezkoder.com/jwt-json-web-token/) OR [video](https://www.youtube.com/watch?v=S20PCL9e_ks)

---

## <a id="validateAPI"> Validate API With JWT </a>
Q. [Validate API With JWT](https://medium.com/@prashantramnyc/authenticate-rest-apis-in-node-js-using-jwt-json-web-tokens-f0e97669aad3)

---

## <a id="eventIndex"> Event Emitter and Events </a>
The EventEmitter is a class that facilitates communication/interaction between objects in Node.js. 
The EventEmitter class can be used to create and handle custom events.

Many of Node's built-in modules inherit from EventEmitter like streams. 
An emitter object basically has two main features:

	Emitting name events. (emit)
	Registering and unregistering listener functions. (on)
 
 Building Blocks:

.emit() - this method in event emitter is to emit an event in module

.on() - this method is to listen to data on a registered event in node.js

.once() - it listen to data on a registered event only once.

.addListener() - it checks if the listener is registered for an event.

.removeListener() - it removes the listener for an event.

[event emitter with streams](https://www.scaler.com/topics/nodejs/event-emitter-nodejs/)

---

## <a id="middlewareIndex"> Middleware </a>
Q. [Middleware & Types](https://selvaganesh93.medium.com/how-node-js-middleware-works-d8e02a936113)

---
## <a id="microIndex"> Microservices </a>
Q. [How to create a microservices in Node.js](https://www.turing.com/kb/how-to-build-microservices-with-node-js)

---

## <a id="securityIndex"> Secure Rest API </a>
Q. [How to create a secure REST API in Node.js](https://www.turing.com/kb/build-secure-rest-api-in-nodejs)

```diff
- A. Make use of HTTPS

HTTPS is the HTTP protocol's secure version. It enables encrypted communication between the client
 and the server, safeguarding data against interception and tampering.

HTTPS is required for any API that handles sensitive data or requires authentication. It ensures that
data is transmitted securely and cannot be intercepted by any malicious activity.
Here, S stands for Security Socket Layer (SSL) to establish encryption in communication

- B. Make use of Authentication

Authentication is a method of verifying a user's identity. It is a critical security measure for any API
because it prevents unauthorized access to sensitive data.

You can use a variety of authentication methods in Node.js, including OAuth, JWT, and API keys.
Each of these methods has advantages and disadvantages, so it is critical to select the best
one for your application.

- C. Restrict access

Another important security measure is to restrict API access. It ensures that the API and its data are
only accessible to authorized users.

You can use access control lists (ACLs) in Node.js to specify who can access the API and what they can do.
 This enables you to limit access to only those users who have the required permissions.

- D. Validation of input

Input validation ensures that the data sent to an API is correct and secure.
It guards against malicious users sending malicious or incorrect data to the API.

To perform input validation in Node.js, you can use a library likes Joe Module, express-validator.
This library enables you to validate API data and reject requests that do not meet the specified criteria.

- E. Implement security best practices

Finally, while developing a REST API in Node.js, it is critical to adhere to security best practices.
This includes implementing secure coding practices such as input validation, HTTPS, and
authentication and authorization.

To securely store passwords, we will use the bcrypt library, and to authenticate users,
we will use the JSON web token library.
We will also interact with a MongoDB database using the Mongoose library.

- Conclusion
To summarize, creating a secure API with Node.js necessitates attention to detail and the implementation of various
security measures to protect against common threats. These safeguards include encrypting and hashing sensitive data,
validating and sanitizing input, implementing authentication and authorization, and keeping software up to date with
the most recent security patches. Regular security testing and having a plan in place for responding to security
incidents are also essential for keeping the API secure.
```
[Back to Top](#nodeJSIndex)

---
``` diff
## <a id="architectureIndex"> Architecture And Event Loop Phases </a>
Q. [NodeJs Architecture](https://www.digitalocean.com/community/tutorials/node-js-architecture-single-threaded-event-loop)

+ Node JS Platform
    Node JS Platform uses “Single Threaded Event Loop” architecture to handle multiple concurrent clients. 
    Then how it really handles concurrent client requests without using multiple threads.
    What is Event Loop model? We will discuss these concepts one by one. 
    Before discussing “Single Threaded Event Loop” architecture,
    first we will go through famous “Multi-Threaded Request-Response” architecture.

+ Traditional Web Application 
Any Web Application developed without Node JS, typically follows “Multi-Threaded Request-Response” model. 

Steps:
     •	Web Server internally maintains a Limited Thread pool to provide services to the Client Requests.
     •	Clients Send request to Web Server.
     •	When Client send requests, Web Server receives those requests. 
     •	Web Server pickup one Client Request
     •	Pickup one Thread from Thread pool
     •	Assign this Thread to Client Request
     •	This Thread will take care of reading Client request, processing Client request,
       performing any Blocking IO Operations (if required) and preparing Response
     •	This Thread sends prepared response back to the Web Server
     •	Web Server sends this response to the respective Client.

Here, Server waits for client request and performs all sub-steps as mentioned above for all n clients. 
That means this model creates one Thread per Client request. If more clients requests require Blocking IO Operations,
then almost all threads are busy in preparing their responses. Then remaining clients Requests should wait for longer time.

+ Drawbacks
     •	Handling more and more client’s request is bit tough.
     •	When client requests increases, then Sometimes, Client’s Request should wait for available threads to process their requests.
     •	Wastes time in processing Blocking IO Tasks.

- Node JS Architecture - Single Threaded Event Loop

    Node JS Platform does not follow above Multi-Threaded Model. 
    It follows Single Threaded with Event Loop Model.
    Node JS Processing model mainly based on Javascript Event based model with Javascript callback mechanism

    + Single Threaded Event Loop Model Processing Steps:
    
    •	Clients Send request to Web Server.
    •	Node JS Web Server internally maintains a Limited Thread pool to provide services to the Client Requests.
    •	Node JS Web Server receives those requests and places them into a Queue. It is known as “Event Queue”.
    •	Node JS Web Server internally has a Component, known as “Event Loop”. 
    •	Event Loop uses Single Thread only. 
    •	Even Loop checks any Client Request is placed in Event Queue. If no, then wait for incoming requests
    •	If yes, then pick up one Client Request from Event Queue
    •	Starts process that Client Request
    •	If that Client Request Does Not requires any Blocking IO Operations, then process everything,
      prepare response and send it back to client.
    •	If that Client Request requires some Blocking IO Operations like interacting with Database, File System,
      External Services then it will follow different approach
    •	Checks Threads availability from Internal Thread Pool
    •	Picks up one Thread and assign this Client Request to that thread.
    •	That Thread is responsible for taking that request, process it, perform Blocking IO operations,
      prepare response and send it back to the Event Loop
    •	Event Loop in turn, sends that Response to the respective Client.

Although we have Multiple Threads in the Background, Node is still said to be Single Threaded since all the Requests are
Received on the Single Thread and Node internally manages the Execution of blocking requests  on the Background Threads.

+ Single Threaded Event Loop Advantages
    1.	Handling more and more concurrent client’s request is very easy.
    2.	Even though our Node JS Application receives more and more Concurrent client requests, there is no need of creating more
       and more threads, because of Event loop.
    3.	Node JS application uses less Threads so that it can utilize only less resources or memory

```

---

```diff
## <a> Event Loop Phases </a>
Q. [Event Loop Phases](https://javascript.plainenglish.io/node-js-event-loop-explained-d27647ec8d53)

- The 6 phases of the Node.js event
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

- The 6 phases
When Node.js finished executing the index.js in the main thread, the Node.js event loop starts to execute the callbacks registered during the main thread’s runtime.

![image](https://github.com/shridharssg/nodejs/assets/139750756/9bff4dab-8d5a-43a1-aa30-908803ca2e47)


- Callback queue
There’s a callback queue in each phase that stores callbacks to be executed in that phase. It’s very similar to the Task queue provided by a browser.

- Timers
This is the first phase in the event loop. It finds expired timers in every iteration (also known as Tick) and executes the timers callbacks created by setTimout and setInterval.

For example:

setTimeout(() => console.log('Timeout 1'), 0);
setTimeout(() => console.log('Timeout 2'), 10);

Timeout 1 will be printed in the first iteration because the first timer expires after 0ms. 
However, Timeout 2 will be printed in another iteration (not necessarily in the second iteration) because the second timer expires after 10ms.

- Pending callbacks
It handles I/O callbacks deferred to the next iteration, such as handling TCP socket connection error.

- Idle, prepare
It’s only used internally.

- Poll
The Poll phase calculates the blocking time in every iteration to handle I/O callbacks. In this phase, the epoll_wait() system call is invoked (in Linux).

- Check
This phase handles the callbacks scheduled by setImmediate(), and the callbacks will be executed once the Poll phase becomes idle.

- Close Callback
This phase handles callbacks if a socket or handle is closed suddenly and the ‘close’ event will be emitted.

- The microtask queue
The microtask queue stores microtasks (callbacks) created by:

process.nextTick()
then() and catch() that handle resolved and rejected promises
Microtasks are executed after the main thread and each phase of the event loop. Microtasks created by process.nextTick() are executed before those created by then() and catch().

![image](https://github.com/shridharssg/nodejs/assets/139750756/5dfe59cb-406e-4c66-8a3e-44091a5c9cf0)

```

[Back to Top](#nodeJSIndex)


---


``` diff

## <a id="fsIndex"> File System </a>

Q. [Node.js file system: ](https://www.w3schools.com/nodejs/nodejs_filesystem.asp)

To handle file operations like creating, reading, deleting, etc., Node.js provides an inbuilt module called FS (File System). 
All file system operations can have synchronous and asynchronous forms depending upon user requirements.

To use this File System module, use the require() method:
var fs = require('fs');

- Common use for File System module:
•	Read Files
•	Write Files
•	Append Files
•	Close Files
•	Delete File

- Read Files
The fs.readFile() method is used to read files on your computer.

var http = require('http');
var fs = require('fs');
http.createServer(function (req, res) {
  fs.readFile('demofile1.html', function(err, data) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write(data);
    return res.end();
  });
}).listen(8080);

- Create Files
The File System module has methods for creating new files:

fs.appendFile()
fs.open()
fs.writeFile()

+ The fs.appendFile() method appends specified content to a file. If the file does not exist, the file will be created:

fs.appendFile('mynewfile1.txt', 'Hello content!', function (err) {
  if (err) throw err;
  console.log('Saved!');
});

+ The fs.open() method takes a "flag" as the second argument, if the flag is "w" for "writing", the specified file is opened for writing. If the file does not exist, an empty file is created:

fs.open('mynewfile2.txt', 'w', function (err, file) {
  if (err) throw err;
  console.log('Saved!');
});

+ The fs.writeFile() method replaces the specified file and content if it exists. If the file does not exist, a new file, containing the specified content, will be created:

fs.writeFile('mynewfile3.txt', 'Hello content!', function (err) {
  if (err) throw err;
  console.log('Saved!');
});

- Delete Files
+ The fs.unlink() method deletes the specified file:

fs.unlink('mynewfile2.txt', function (err) {
  if (err) throw err;
  console.log('File deleted!');
});

- Rename File
+ The fs.rename() method renames the specified file:
+ Here, Rename "mynewfile1.txt" to "myrenamedfile.txt":

fs.rename('mynewfile1.txt', 'myrenamedfile.txt', function (err) {
  if (err) throw err;
  console.log('File Renamed!');
});

```

``` diff

## <a id="dataIndex"> Data Stream </a>

Streams are objects that let you read data from a source or write data to a destination in continuous fashion.
Streaming means listening to music or watching video in real time, instead of downloading a file to your computer
and watching it later

+ There are four types of streams
    •	Readable − Stream which is used for read operation.
    •	Writable − Stream which is used for write operation.
    •	Duplex − Stream which can be used for both read and write operation.
    •	Transform − A type of duplex stream where the output is computed based on input.

Each type of Stream is an EventEmitter instance and throws several events at different instance of times.

+ Methods:
   •	data ** − This event is fired when there is data is available to read.
   •	end − This event is fired when there is no more data to read.
   •	error − This event is fired when there is any error receiving or writing data.
   •	finish − This event is fired when all the data has been flushed to underlying system.

[Stream Example](https://www.freecodecamp.org/news/node-js-streams-everything-you-need-to-know-c9141306be93/)

```
[Back to Top](#nodeJSIndex)
