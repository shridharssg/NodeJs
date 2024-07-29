imp - https://medium.com/@akanksha17/getting-started-with-writing-api-tests-in-node-js-96eb2c694cad

JS testing with Mocha : https://medium.com/practical-software-testing/javascript-testing-with-mocha-a-series-f4bfcab26532

API Testing with Mocha and Chai: https://medium.com/practical-software-testing/api-testing-with-mocha-and-chai-944c2f26c340

GitHub: https://github.com/damian-sketch/Mocha/tree/master

---

**Mocha** 
It is an open-source Javascript test framework that runs on Nodejs and on the browser and is used for testing Synchronous and Asynchronous code.

Mocha provides functions that are executed in a specific order and logs their results to a terminal window. Mocha uses hooks to organize its structure. 

They include:

**describe():** Used to group one or more test cases and can be nested.

**it():** the test case is laid down here

**before():** It’s a hook to run before the first it() or describe();

**beforeEach():** It’s a hook to run before each it() or describe();

**after():** It’s a hook to run after it() or describe();

**afterEach():** It’s a hook to run after each it() or describe();

The keywords describe and it provide structure to the tests by grouping them into Test cases and Test suites. 
A Test case is a set of actions executed to validate a feature or functionality of your software.
A Test Suite is a collection of tests all relating to a similar functionality or behavior.

---

**Chai**
Mocha can be used with any assertion library but use Chai, a popular assertion library for Node.js and the browser. 
It provides functions and methods which help you compare the output of a test with the expected result. 
Chai provides clean syntax that reads almost like English. 

An example of a Chai assertion is 

**expect(exampleArray).to.have.lengthOf(3);**

This simply validates whether the variable ‘exampleArray’ has a length of 3 or not.

---

**Setting up Mocha and Chai**

npm i — g mocha

npm install chai
```
{
  "scripts": {
    "test": "mocha"
  }
}
```

Writing tests using Mocha and Chai

Now the Setup is all done, let us create a test case of A calculator which will perform the basic arithmetic operations:

**Create a file src/calculator.js and add the following code:**

```
const add = (a, b) => a + b
const subtract = (a, b) => a - b
const multiply = (a, b) => a * b
const divide = (a, b) => b !== 0 ? (a / b) : undefined module.exports = {
  add,
  subtract,
  multiply,
  divide,
}
```

Create a test.js file inside your test directory and add the following code:

```
const chai = require('chai')
const expect = chai.expect
const calculator = require('../src/calculator')
describe('Calculator', () => {
  describe('Addition', () => {
    it('should sum two numbers', () => {
        expect(calculator.add(2, 2)).to.equal(4)
        expect(calculator.add(50, 39)).to.equal(89)
      })
  })
  describe('Subtraction', () => {
     it('should subtract two numbers', () => {
         expect(calculator.subtract(6, 2)).to.equal(4)
         expect(calculator.subtract(50, 39)).to.equal(11)
         })
   })
  describe('Multiplication', () => {
     it('should multiply two numbers', () => {
         expect(calculator.multiply(3, 2)).to.equal(6)
         expect(calculator.multiply(-31, 32)).to.equal(-992)
         expect(calculator.multiply(-5, -2)).to.equal(10)
        })
   })

describe('Division', () => {
   it('should divide two numbers', () => {
       expect(calculator.divide(4, 2)).to.equal(2)
       expect(calculator.divide(50, 5)).to.equal(10)
    })
   it('should return NaN if the denominator is zero', () => {
      expect(calculator.divide(4, 0)).to.equal(undefined)
      expect(calculator.divide(-15, 0)).to.equal(undefined)
      })
  })
})

**Note that under Division, we have nested two it functions**
```

You can now run your test case using:
npm test

---

**API Testing with Mocha and Chai**

In this article, we will add to our list of dependencies. Make sure to have these installed first:

**npm i body-parser lowdb uuid supertest express**

**lowdb**: This is a small JSON database for node, electron, and the browser. Suitable for a small project like this one

**supertest**: A library for HTTP assertions

Next, we will need to have the API in place so go ahead and create an api.js file inside our test folder and in it, paste this code:

```
var express = require('express');
var lowdb = require('lowdb');
var filesync = require('lowdb/adapters/FileSync');
var { v4: uuidv4 } = require('uuid');
var bodyParser = require('body-parser');

// Instantiate the app
var app = express();

// Load the lowdb adapter to allow read/write functionality on the db file
var adapter = new filesync('db.json');
var db = lowdb(adapter);

// Set the default structure for the db file
db.defaults({
    posts: [],
}).write();

//Parse incoming request data
app.use(bodyParser.json());

// Routes for the API
//Adding a task
app.post('/add-task', function(req, res){
   var title = req.body.title;
   var id = uuidv4();
   if (!title || title === undefined){
     res.status(400).end()
   }
   else{
    db.get('posts').push({ id, title }).write();
   return res.status(201).end();
   }
   
});


//Listing all tasks
app.get('/tasks', function(req, res){
  return res.json(db.getState());
});

//Listing a particular task
  app.get('/tasks/:id', function(req,res){
    var id = req.params.id;
    let a = db.get('posts').find({ id: id });
    if (a) {
        return res.json(a);
      }
      return res.status(404).end();
});


//Update a task
app.put('/tasks/:id', function(req,res){
    var update = req.body.title;
    if (!update || update === undefined){
      res.status(400).end()
    }
    else{
      db.get('posts').find({id : req.params.id}).assign({title : update}).write();
      return res.status(200).end();
    }
    
})

// Delete task
app.delete('/tasks/:id', function(req,res){
    db.get('posts').remove({ id : req.params.id}).write();
    return res.status(200).end();
})

// API server listing port 3000
app.listen(3000, function() {
    console.log('API up and running');
  });

module.exports = app;

```

let us now write our API tests. In your test.js file, load these first:

```

const chai = require('chai')
const expect = chai.expect
const request = require('supertest');
const app = require('./api')    // above code to check API result

// API tests
describe('POST /add-task', function() {
	it('Adds a task', function(done) {
	  request(app)
		.post('/add-task')
		.send({ title: "API testing rocks!" })
		.expect(201, done);
	});
  });

  describe('GET /tasks', function() {
	it('List all tasks', function(done) {
	  request(app)
		.get('/tasks')
		.expect(200, done);
	});
  });

  describe('GET /tasks/:id', function() {
	it('Gets a particular task', function(done) {
	  request(app)
		.get('/tasks/957095a3-fff0-47a5-9d2f-d64fddbd67e2')
		.expect(200, done);
	});
  });


  describe('PUT /tasks/:id', function() {
	it('Updates a particular task', function(done) {
	  request(app)
		.put('/tasks/957095a3-fff0-47a5-9d2f-d64fddbd67e2')
		.send({ title : "Updated task buoy" })
		.expect(200, done);
	});
  });  


  describe('DELETE /tasks/:id', function() {
	it('Deletes a particular task', function(done) {
	  request(app)
		.delete('/tasks/8e88212c-2a05-4774-a371-9638cc897e52')
		.expect(200, done);
	});
  });

```
Now when you run npm test in your terminal, it will give the calculator results

So In this way, a simple REST API built with express and tested using Mocha, Chai, and Supertest. 

