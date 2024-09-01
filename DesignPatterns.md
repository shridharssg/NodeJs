What are design patterns?
  - A design pattern is a general, reusable solution to a commonly occurring problem.
  - Applying design patterns tailored to Node.js can lead to more efficient and optimized applications
  - developers can create more robust, maintainable, and extensible codebases.

1. Singleton Pattern
2. Observers
3. Factories
4. Dependency Injection
5. 

---

### Singleton Pattern

  The singleton patterns restrict the number of instantiations of a “class” to one. Creating singletons in Node.js is pretty straightforward, as require is there to help you.
  It does not matter how many times you will require this module in your application; it will only exist as a single instance.
  
```
  //area.js
var PI = Math.PI;

function circle (radius) {
  return radius * radius * PI;
}

module.exports.circle = circle;
```

app.js

```
var areaCalc = require('./area');

console.log(areaCalc.circle(5));
```

In Node.js, the `require` function uses the Singleton pattern to manage modules. Here's how it works:

1. When you `require` a module for the first time, Node.js loads the module and caches it in memory.

2. Subsequent calls to `require` the same module return the cached instance, rather than creating a new one.

This ensures that:
- Modules are only loaded once, reducing overhead and improving performance.
- All parts of the application share the same instance of the module, maintaining consistency and avoiding conflicts.

To illustrate this, consider the following example:

```
// myModule.js

console.log('Module loaded');

module.exports = {
  foo: 'bar'
};

// app.js
const myModule1 = require('./myModule');
const myModule2 = require('./myModule');

console.log(myModule1 === myModule2); // true

```

In this example, `myModule` is loaded only once, and both `myModule1` and `myModule2` reference the same instance.

Node.js's `require` function uses a cache object (`require.cache`) to store loaded modules. When you `require` a module, Node.js checks the cache first and returns the cached instance if available. If not, it loads the module, caches it, and returns the new instance.

**If you use `require` to load `myModule` in different files, it will still be loaded only once.**

Node.js's `require` cache is global, meaning it spans across all files in your application. When you `require` a module, Node.js checks the cache first. If the module is already cached, it returns the cached instance. If not, it loads the module, caches it, and returns the new instance.

Here's an example:
```
// file1.js

const myModule = require('./myModule');

console.log(myModule.foo); // bar

// file2.js

const myModule = require('./myModule');

console.log(myModule.foo); // bar

```

In this example, even though `myModule` is `require` in two different files (`file1.js` and `file2.js`), it is still loaded only once. Both files share the same instance of `myModule` from the cache.

This behavior applies to all files in your application, as long as they are part of the same Node.js process. If you run multiple Node.js processes (e.g., using `child_process` or separate terminal windows), each process will have its own cache, and the module will be loaded separately in each process.

---

