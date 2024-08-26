# <a id="nodeJSIndex"> NodeJs </a>

Q. [Promises and Types](#promisesIndex)

Q. [Async Await](#asyncAwait)

Q. [Event Emitter and Events](#eventIndex)

Q. [File System](#fsIndex)

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
