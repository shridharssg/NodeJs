# <a id="nodeJSIndex"> NodeJs </a>

Q. [Promises and Types](#promisesIndex)

Q. [Async Await](#asyncAwait)

Q. [Json Web Token](#jsonWebTokenIndex)

Q. [Validate API with JWT](#validateAPI)

Q. [Event Emitter and Events](#eventIndex)

Q. [Middleware and Types](#middlewareIndex)

Q. [Microservices](#microIndex)

Q. [Secure Rest API](#securityIndex)

Q. [File System](#fsIndex)

Q. [Data Stream](#dataIndex)

Q. [Clutser](#cluster)

---
## <a id="promisesIndex"> Promises </a>
Q. [Promise](https://dotnettutorials.net/lesson/javascript-promise/)
Q. [Promise Chaining](https://dotnettutorials.net/lesson/promise-chaining-in-javascript/)
Q. [Promise Types](https://dotnettutorials.net/lesson/javascript-promise-race-vs-promise-all/)  

promise.race() and promise.all()

---

## <a id="asyncAwait"> Async Await </a>
Q. [Async Await](https://dotnettutorials.net/lesson/javascript-async-await/)

to deal with asynchronous operations, we often used the callback functions. However, when we nest many callback functions, the code will be more difficult to maintain. And we end up with an unmanageable code issue which is known as the callback hell. So, to overcome this Promise came to rescue.

ES2017 introduced async and await build on top of promises and generators to write asynchronous actions inline. This makes asynchronous or callback code much easier to maintain and more readable.

```
async function f() {
    //Promise code goes here
    let value = await promise;// works only inside async functions
    return 1;
}
```

The keyword await, as the name describes it makes JavaScript wait until that promise settles and returns its result. It can be used within the async block only. It only makes the async block wait.

A function which is defined as async is a function that can perform asynchronous actions but still look synchronous. 

---

## <a id="jsonWebTokenIndex"> Json Web Token </a>
Q. [JSON Web Token](https://www.bezkoder.com/jwt-json-web-token/) OR [video](https://www.youtube.com/watch?v=S20PCL9e_ks)

---

## <a id="validateAPI"> Validate API With JWT </a>
Q. [Validate API With JWT](https://medium.com/@prashantramnyc/authenticate-rest-apis-in-node-js-using-jwt-json-web-tokens-f0e97669aad3)
Q. [Refresh Token vs Access Token](https://medium.com/@techsuneel99/jwt-authentication-in-nodejs-refresh-jwt-with-cookie-based-token-37348ff685bf)
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

next () : Pass control to the next middleware or route handler
```
const normalMiddleware = (req, res, next) => {
  // Perform some operations
  console.log('Executing normal middleware');
  next(); // Pass control to the next middleware or route handler
};
```
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
---

What is Buffer in NodeJS?
Buffer is a temporary memory, mainly used by the stream to hold some data until consumed. Buffer is mainly used to store binary data while reading from a file or receiving packets over the network.

It represents a fixed-length sequence of bytes. It allocated memory outside of the V8 heap. (Buffer not available in browser’s JavaScript)

A simple example of converting a string into a buffer:

// a string
const str = "Hey. this is a string!";
// convert string to Buffer
const buff = Buffer.from(str, "utf-8");
console.log(buff); // <Buffer 48 65 79 2e ... 72 69 6e 67 21>
To convert buffer into a string:

// if the buffers contain text
buffer.toString(encoding) // encoding = 'utf-8'
// if you know how many bytes the buffer contains then
buffer.toString(encoding, 0, numberOfBytes) // numberOfBytes = 12

---

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
