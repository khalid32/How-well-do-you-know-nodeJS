# How-well-do-you-know-nodeJS

## Description
A list of specific questions by Samer Buna a Node.js developer is expected to answer

## Link
[Learning the Node.js Runtime :: Node.js Beyond the Basics](https://jscomplete.com/learn/node-beyond-basics)

## Table of Content

- [1. How come when you declare a global variable in any Node.js file it’s not really global to all modules?](https://github.com/khalid32/How-well-do-you-know-nodeJS#1-how-come-when-you-declare-a-global-variable-in-any-nodejs-file-its-not-really-global-to-all-modules)
- [2. When exporting the API of a Node module, why can we sometimes use `exports` and other times we have to use `module.exports`?](https://github.com/khalid32/How-well-do-you-know-nodeJS#2-when-exporting-the-api-of-a-node-module-why-can-we-sometimes-use-exports-and-other-times-we-have-to-use-moduleexports)
- [3. Can we require local files without using relative paths?](https://github.com/khalid32/How-well-do-you-know-nodeJS#3-can-we-require-local-files-without-using-relative-paths)
- [4. What is the Event Loop? Is it part of V8?](https://github.com/khalid32/How-well-do-you-know-nodeJS#3-can-we-require-local-files-without-using-relative-paths)
- [5. What is the Call Stack? Is it part of V8?](https://github.com/khalid32/How-well-do-you-know-nodeJS#3-can-we-require-local-files-without-using-relative-paths)
- [6. What is the difference between `setImmediate` and `process.nextTick`?](https://github.com/khalid32/How-well-do-you-know-nodeJS#3-can-we-require-local-files-without-using-relative-paths)
- [7. How do you make an asynchronous function return a value?](https://github.com/khalid32/How-well-do-you-know-nodeJS#7-how-do-you-make-an-asynchronous-function-return-a-value)
- [8. Can callbacks be used with promises or is it one way or the other?](https://github.com/khalid32/How-well-do-you-know-nodeJS#8-can-callbacks-be-used-with-promises-or-is-it-one-way-or-the-other)
- [9. What are the major differences between spawn, exec, and fork?](https://github.com/khalid32/How-well-do-you-know-nodeJS#9-what-are-the-major-differences-between-spawn-exec-and-fork)
- [10. How does the cluster module work? How is it different than using a load balancer?](https://github.com/khalid32/How-well-do-you-know-nodeJS#10-how-does-the-cluster-module-work-how-is-it-different-than-using-a-load-balancer)
- [11. What are the --harmony flags?](https://github.com/khalid32/How-well-do-you-know-nodeJS#11-what-are-the---harmony-flags)
- [12. How can you read and inspect the memory usage of a Node.js process?](https://github.com/khalid32/How-well-do-you-know-nodeJS#12-how-can-you-read-and-inspect-the-memory-usage-of-a-nodejs-process)
- [13. Can reverse-search in commands history be used inside Node’s REPL?](https://github.com/khalid32/How-well-do-you-know-nodeJS#13-can-reverse-search-in-commands-history-be-used-inside-nodes-repl)

### 1. How come when you declare a global variable in any Node.js file it’s not really global to all modules?
A module's code is wrapped by a function wrapper that looks like the following
```
(function(exports, require, module, __filename, __dirname){
    // module code
})
```
This wrapping allows to keeps top-level variables (defined with var, const or let) scoped to the module, rather than to the global object.
In node, the top-level scope is not the golbal scope; `var something` inside a Node module will be **local to that module**.

### 2. When exporting the API of a Node module, why can we sometimes use `exports` and other times we have to use `module.exports`?
To understand the difference, we can look at this simplified view of a JavaScripe file in Node.js:
```
var module = { exports: {} };
var exports = module.exports;

// your code

return module.exports;
```

so, `exports` is initially an alias to `module.exports`. If you want to simply export an object with named fields, you can use the `exports` shortcut. For example, had we written `exports.a = 9`, we'd actually export this object: `{a: 9}`.

However, if you want to export a function or another object, you have to use the `module.exports` object. For example: `module.exports = function bar(){}`. Once you do that, `exports` and `module.exports` no longer reference the same object.

### 3. Can we require local files without using relative paths?
There are several options, but my favourites are these:

###### 1. The Alias
- Install the [module-alias](https://www.npmjs.com/package/module-alias) package
```
npm i --save module-alias
```
- Add paths to your `package.json` like this:
```
{
    "_moduleAliases": {
        "@lib": "app/lib",
        "@models": "app/models"
    }
}
```
- In your entry-point file, before any `require()` calls:
```
require('module-alias/register')
```
- You can now require files like this:
```
const Article = require('@models/article');
```

###### 2. The Global
- In your entry-point file, before any `require()` calls:
```
global.__base = __dirname + '/';
```
- In your very/far/away/module.js
```
const Article = require(`${__base}app/models/article`);
```

###### 3. The Module
- Install some module:
```
npm install app-module-path --save
```
- In your entry-point file, before any `require()` calls:
```
require('app-module-path').addPath(`${__dirname}/app`);
```
- In your very/far/away/module.js
```
const Article = require('models/article');
```

### 4. What is the Event Loop? Is it part of V8?
In event-driven programming, an application expresses interest in certain events and respond to them when they occur. This is the way Node.js can handle asynchronous execution while running the code in a single thread. When an asynchronous operation starts (for example, when we call `setTimeout`, `http.get` or `fs.readFile`), Node.js sends these operations to a different thread allowing V8 to keep executing our code. Node also calls the callback when the counter has run down or the IO/http operation has finished. In Node.js, the responsibility of gathering events from the operating system or monitoring other sources of events is handled by [libuv](https://github.com/libuv/libuv), and the user can register callbacks to be invoked when an event occurs. The event-loop usually keeps running forever.

> the event loop is something that embedders should have control over. However, it is also a fundamental abstract concept of the JavaScript programming model. V8's solution is to provide a default implementation that embedders can override. See [Relationship between event loop,libuv and v8 engine.](https://stackoverflow.com/questions/49811043/relationship-between-event-loop-libuv-and-v8-engine)

###### SUB QUES: Does the event loop and JavaScript code is running in the same thread?
Effectively **YES**. the "event loop" isn't really a thing that's running. It's mostly just a queue of callbacks waiting for their turn to run.

### 5. What is the Call Stack? Is it part of V8?

The call stack is the basic mechanisum for javascript code execution. When we call a function, we push the function parameters and the return address to the stack. This allows to runtime to know where to continue code execution once the function ends.
In NodeJS, the Call Stack is handled by V8.

[Understanding the JavaScript call stack](https://medium.freecodecamp.org/understanding-the-javascript-call-stack-861e41ae61d4)

The call stack is primarily used for `function invocation(call)`. Since the call stack is single, function(s) execution is done one at a time from top to bottom. means **the call stack is synchronous**.

> A call stack is a data structure that uses the **Last In, First Out(LIFO)** principle to temporarily store and manage `function invocation(call)`.

###### Temporarily Store
When a function is invoked(called), the function, its parameters and variables are pushed into the call stack to form a stack frame. This stack is a memory location in the stack. The memory is cleared when the function returns as it is pop out of the stack.
![Temporarily Store](https://user-images.githubusercontent.com/8571179/54331221-348e3980-4643-11e9-9597-6b0f99e977f7.png)

### 6. What is the difference between `setImmediate` and `process.nextTick`?
`setImmediate` and `process.nextTick` are two options to postpone code execution.

###### setImmediate
```
setImmediate(function(){
    console.log('PrintIn');
})
```
First of all, `callback` function gets internally queued. Next, a check handle is registered in the event loop; its associated callback is a simple function that runs all the queued `callback` functions. Therefore, whenever the loop hits the check handles part, the internal queue is emptied and all the `callback`s are executed.

The whole point of `setImmediate` is to postpone the execution of code until immediate after polling for I/O.

> `setImmediate` callbacks will be the last to run on the current event loop iteration.

###### process.nextTick
In general, `process.nextTick` schedules a function to be executed when the current *tick* ends.
However, in order to dig deper, we first need to be familiar with one of **node's central** C++ function, `MakeCallback`, which takes as parameter a JavaScript function along with its arguments and its calling object(*i.e.* the value of `this` inside the function)

*[Node.js: setImmediate vs. process.nextTick](http://plafer.github.io/2015/09/08/nextTick-vs-setImmediate/)*

`setImmediate` queues a function behind whatever I/O event callbacks that are already in the event queue. `process.nextTick` queues a function at the head of the event queue so that it executes immediately after the currently running function completes.

### 7. How do you make an asynchronous function return a value?
> Note that, it is not possible to return from an asynchronous call inside a synchronous method.

you need to pass a callback that will receive the return value.
```
function foo(address, fn){
    geocoder.geocode({'address': address}, function(result, status){
        fn(result[0].geometry.location);
    })
}

foo("address", function(location){
    alert(location);
});
```
**The thing is, if an inner function call is asynchronous, then all the functions 'wrapping' this call must also be asynchronous in order to 'return' a response.**

> You could return a promise resolving to that value, for example `return Promise.resolve(true)`.

### 8. Can callbacks be used with promises or is it one way or the other?
Callbacks are not interchangeable with Promises. This means that callback-based APIs cannot be used as Promises. The main difference with callback-based APIs is it does not return a value, it just executes the callback with the result.

But in NodeJS, both can be used together. For example, the following method calls a callback and returns a promise:
```
function foo(cb){
    // do some processing
    if(cb){
        cb();
    }
    return Promise.resolve(true);
}
```

### 9. What are the major differences between `spawn`, `exec`, and `fork`?
- `exec` methods spawns a shell and then executes a command within that shell, buffering any generated output.
- `spawn` works similarly to `exec`. The main difference is that `spawn` returns the process output as a stream while `exec` returns it as a buffer.
- `fork` is a special case of `spawn` that also creates a new V8 engine instance. This is useful to create additional workers of the same Node.js code base.
> The `child_process.fork()` method is a special case of `child_process.spawn()` used specifically to spawn new Node.js processes. Like `child_process.spawn()`, a **ChildProcess** object is returned. The returned **ChildProcess** will have an additional communication channel built-in that allows messages to be passed back and forth netween the parent and child.

[Understanding execFile, spawn, exec, and fork in Node.js ](https://dzone.com/articles/understanding-execfile-spawn-exec-and-fork-in-node)

### 10. How does the cluster module work? How is it different than using a load balancer?
> The cluster module is a NodeJS nodule that contains a set of functions and properties that help us forking processes to take advantage of multi-core systems. **It is probably the first level of scalability you must take care in your node application, specifically if you are working in a HTTP server application, before going to a higher scalability levels(meaning that scaling vertically and horizontally in different machines).**
> Cluster lets you set up a master process that can handle load to worker processes.
The cluster module allow us improve performance of our application in multicore CPU systems.
> The cluster module allow us to load balance the incoming request among a set of worker processes and, because of this, improving the throughput of our application.

[Understanding the NodeJS cluster module](http://www.acuriousanimal.com/2017/08/12/understanding-the-nodejs-cluster-module.html)

The [cluster module](https://nodejs.org/api/cluster.html) works by forking the server into several worker processes(all run inside the same host). The master process listens and accepts new connections and distributes them across the worker processes in a round-robin fashion (with some build-in smarts to avoid overloading a worker process).
A load balancer, in contrast, is used to distribute incoming connections across *multiple hosts*.

### 11. What are the `--harmony` flags?
`--harmony` is a shortcut to enable all the harmony features (e.g. `--harmony_scoping`, `--harmony_proxies` etc). `harmony` enables new ECMAScript 6 features in the language.

V8 is constantly improving and typically ships with features which may not be stable or ready for production environments, that require command-line **flags** to enable. Those **flags** are the **harmony flags**.

These are flags that one can pass to the Node.js runtime to enable **Staged** features. Stages features are almost-completed features that are not considered stable by the V8 team.

### 12. How can you read and inspect the memory usage of a Node.js process?
###### Heap:
The heap is a memory segment used to store objects, strings and closure

###### Resident Set:
A running Node.js process store all its memory inside a **Resident Set**. You can think of it as of a big box which contains some more boxes. The *Resident Set* contains also the actual JavaScript code(inside the **Code segment**) and the **Stack**, where all the variables live.
![node-js-memory-usage-768x432](https://user-images.githubusercontent.com/8571179/55133956-9bd2e000-5151-11e9-98e8-7ea5a4737969.png)

###### process!:
**process** is a global Node.js object which contains information about the current Node.js process, and it provides **memoryUsage()** method.

###### memoryUsage():
memoryUsage returns an object with various information: **rss, heapTotal, heapUsed** (and **external**)
- **rss** stands for *Resident Set Size*, it is the total memory allocated for the process execution.
- **heapTotal** is the total size of the allocated heap.
- **heapUsed** is the actual memory used during the execution of our process.

You can invoke the `process.memoryUsage()` method which returns an object describing the memory usage of the Node.js process, measured in bytes.

[How to inspect the memory usage of a process in Node.Js](https://www.valentinog.com/blog/memory-usage-node-js/)

### 13. Can reverse-search in commands history be used inside Node’s REPL?
Currently is seems like its not possible. The **Node REPL** does allow to persist the history into a file and later load it, but doesn't allow to reverse-search it. So it appears that reverse history search is not natively supported by the REPL.

However you can install the `rlwrap` utility and run it on top of the Node REPL to provide similar functionality. The [REPL documentation](https://nodejs.org/api/repl.html#repl_using_the_node_js_repl_with_advanced_line_editors) site has some basic instructions to get you up and running.

### 14. What are V8 object and function templates?
V8 object is a native, C++ representation of a JavaScript object. It is a essentially the way the V8 engine views javascript objects. A function template is the blueprint for a single function. You create a JavaScript instance of the template by calling the template's.

`GetFunction` method from within the context in which you wish to instantiate the JavaScript function. You can also associate a C++ callback with a function template which is called when the JavaScript function instance is invoked.

###### FunctionTemplate structure:
Note that: A JavaScript function, is a first class object.
It can be called, can be called as a constructor(instanced), which will create a prototype chain if needed, and the function object itself can hold functions and variables.
All this translates directly into native code, where a `v8::FunctionTemplate` object(or `interface_template`), exposes two methods returning a `Local<ObjectTemplate>`:
```
// prototype function template
Local<ObjectTeplate> prototype_t = interface_t->PrototypeTemplate();

// instance function template
Local<ObjectTemplate> instance_t = interface_t->InstanceTemplate();
```

###### CallHandler:
CallHandler function is a special native constructor delegate. It is responsible for associating a JavaScript with its native wrappable when invoked from javascript as `new Event()`, but it also must handle the situation when an existing native object, just needs to be wrapped and be available in javascript.

[Javascript native wrappers in V8 — Part I](https://medium.com/@hyperandroid/javascript-native-wrappers-in-v8-part-i-67851a3a797a)

### 15. What is libuv and how does Node.js use it?
`libuv` is a library that allows your JavaScript code(via V8) to perform I/O.
whether it is network, file etc. So from TCP level connectivity all the way to file/system ops are actually performed by the libuv library.
It achieves great performance through combining both asynchronous event loop and thread pool fro non-io blocking and io blocking operation. It's a good choice for high performance server.

The only downside is if synchronous thread-pool model is your daily life, you may find the asynchronous model a little tricky, especially you need to know when is the best time to release the "handles"(there are different kind of "handles"), if not getting that right, libuv will crash and make your debugging hard.

libuv is written in C, but it uses some OO(Object Oriented) trick in things like "handle" which is kind of smart.

### 16. How can you make Node’s REPL always use JavaScript strict mode?
If you want to use [strict mode](http://2ality.com/2011/01/javascripts-strict-mode-summary.html) in the [Node.js REPL](https://nodejs.org/api/repl.html), you have two options.

**Option 1.** Start the REPL as usual, wrap your code in an [IIFE](http://2ality.com/2011/02/javascript-variable-scoping-and-its.html):
```
(function(){
    'use strict';
    'abc'.length = 1
}());
// TypeError: Cannot assign to read only property 'length'
```
(Assigning to a read-only property fails silently in sloppy mode.)

**Option 2.** Use the Node.js command line option `--use_strict`:
```
node --use_strict
```
Afterwards, everything you write in the REPL is interpreted in strict mode:
```
'abc'.length = 1
// TypeError: Cannot assign to read only property 'length'
```

### 17. How can we do one final operation before a Node process exits? Can that operation be done asynchronously?
By registering a handler for `process.on('exit')`:
```
function exitHandler(options, err){
    console.log('clean');
}

process.on('exit', exitHandler.bind(null));
```
Listener functions to the `exit` event must only perform synchronous operations. To perform asynchronous opertions, one can register a handler for `process.on('beforeExit')`.