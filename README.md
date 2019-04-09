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
- [14. What are V8 object and function templates?](https://github.com/khalid32/How-well-do-you-know-nodeJS#14-what-are-v8-object-and-function-templates)
- [15. What is libuv and how does Node.js use it?](https://github.com/khalid32/How-well-do-you-know-nodeJS#15-what-is-libuv-and-how-does-nodejs-use-it)
- [16. How can you make Node’s REPL always use JavaScript strict mode?](https://github.com/khalid32/How-well-do-you-know-nodeJS#16-how-can-you-make-nodes-repl-always-use-javascript-strict-mode)
- [17. How can we do one final operation before a Node process exits? Can that operation be done asynchronously?](https://github.com/khalid32/How-well-do-you-know-nodeJS#17-how-can-we-do-one-final-operation-before-a-node-process-exits-can-that-operation-be-done-asynchronously)
- [18. Besides V8 and libuv, what other external dependencies does Node have?](https://github.com/khalid32/How-well-do-you-know-nodeJS#18-besides-v8-and-libuv-what-other-external-dependencies-does-node-have)
- [19. What’s the problem with the process uncaughtException event? How is it different than the exit event?](https://github.com/khalid32/How-well-do-you-know-nodeJS#19-whats-the-problem-with-the-process-uncaughtexception-event-how-is-it-different-than-the-exit-event)
- [20. Do Node buffers use V8 memory? Can they be resized?](https://github.com/khalid32/How-well-do-you-know-nodeJS#20-do-node-buffers-use-v8-memory-can-they-be-resized)
- [21. What’s the difference between Buffer.alloc and Buffer.allocUnsafe?](https://github.com/khalid32/How-well-do-you-know-nodeJS#21-whats-the-difference-between-bufferalloc-and-bufferallocunsafe)
- [22. How is the slice method on buffers different from that on arrays?](https://github.com/khalid32/How-well-do-you-know-nodeJS#22-how-is-the-slice-method-on-buffers-different-from-that-on-arrays)
- [23. What is the string_decoder module useful for? How is it different than casting buffers to strings?](https://github.com/khalid32/How-well-do-you-know-nodeJS#23-what-is-the-string_decoder-module-useful-for-how-is-it-different-than-casting-buffers-to-strings)
- [24. What are the 5 major steps that the require function does?](https://github.com/khalid32/How-well-do-you-know-nodeJS#24-what-are-the-5-major-steps-that-the-require-function-does)
- [25. What is the `require.resolve` function and what is it useful for?](https://github.com/khalid32/How-well-do-you-know-nodeJS#25-what-is-the-requireresolve-function-and-what-is-it-useful-for)
- [26. What is the `main` property in `package.json` useful for?](https://github.com/khalid32/How-well-do-you-know-nodeJS#26-what-is-the-main-property-in-packagejson-useful-for)
- [27. What are circular modular dependencies in Node and how can they be avoided?](https://github.com/khalid32/How-well-do-you-know-nodeJS#27-what-are-circular-modular-dependencies-in-node-and-how-can-they-be-avoided)
- [What is `require`?](https://github.com/khalid32/How-well-do-you-know-nodeJS#important-what-is-require)
- [28. What are the 3 file extensions that will be automatically tried by the require function?](https://github.com/khalid32/How-well-do-you-know-nodeJS#28-what-are-the-3-file-extensions-that-will-be-automatically-tried-by-the-require-function)
- [29. When creating an http server and writing a response for a request, why is the end() function required?](https://github.com/khalid32/How-well-do-you-know-nodeJS#29-when-creating-an-http-server-and-writing-a-response-for-a-request-why-is-the-end-function-required)
- [30. When is it ok to use the file system *Sync methods(like readFileSync)?](https://github.com/khalid32/How-well-do-you-know-nodeJS#30-when-is-it-ok-to-use-the-file-system-sync-methodslike-readfilesync)
- [31. How can you print only one level of a deeply nested object?](https://github.com/khalid32/How-well-do-you-know-nodeJS#31-how-can-you-print-only-one-level-of-a-deeply-nested-object)
- [32. What is the `node-gyp` package used for?](https://github.com/khalid32/How-well-do-you-know-nodeJS#32-what-is-the-node-gyp-package-used-for)
- [33. The objects exports, require, and module are all globally available in every module but they are different in every module. How?](https://github.com/khalid32/How-well-do-you-know-nodeJS#33-the-objects-exports-require-and-module-are-all-globally-available-in-every-module-but-they-are-different-in-every-module-how)
- [34. How can a module be both requirable by other modules and executable directly using the node command?](https://github.com/khalid32/How-well-do-you-know-nodeJS#34-how-can-a-module-be-both-requirable-by-other-modules-and-executable-directly-using-the-node-command)
- [35. What’s an example of a built-in stream in Node that is both readable and writable?](https://github.com/khalid32/How-well-do-you-know-nodeJS#35-whats-an-example-of-a-built-in-stream-in-node-that-is-both-readable-and-writable)
- [36. What’s the difference between using event emitters and using simple callback functions to allow for asynchronous handling of code?](https://github.com/khalid32/How-well-do-you-know-nodeJS#36-whats-the-difference-between-using-event-emitters-and-using-simple-callback-functions-to-allow-for-asynchronous-handling-of-code)
- [37. The `require` function always caches the module it requires. What can you do if you need to execute the code in a required module many times?](https://github.com/khalid32/How-well-do-you-know-nodeJS#37-the-require-function-always-caches-the-module-it-requires-what-can-you-do-if-you-need-to-execute-the-code-in-a-required-module-many-times)
- [38. What’s the difference between the Paused and the Flowing modes of readable streams?](https://github.com/khalid32/How-well-do-you-know-nodeJS#38-whats-the-difference-between-the-paused-and-the-flowing-modes-of-readable-streams)

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

### 18. Besides V8 and libuv, what other external dependencies does Node have?
There are tons of dependencies like `http-parser`, `c-ares`, `OpenSSL`, `zlib`
You should check [NodeJS Dependencies](https://nodejs.org/en/docs/meta/topics/dependencies/)

### 19. What’s the problem with the process `uncaughtException` event? How is it different than the exit event?
The `uncaughtException` event is emitted when an uncaught javascript exception bubbles all the way back to the event loop. Once this event emmits, it means that your application is in an undefined state and it is not safe to continue.
> Hence this event should only be used to perform synchronous cleanup of resources, logging and shutting down the process.

The `exit` event is emitted when the Node.js process is about the exit as a result of either:
- The `process.exit()` method being called explicitly.
- The event loop has no additional work to perform.

### 20. Do Node buffers use V8 memory? Can they be resized?
###### Buffer:
A buffer is a region of a physical memory storage used to temporarily store data while it is being moved from one place to another.

In node, each buffer corresponds to some raw memory allocated outside V8. A buffer acts like an array of integers, but cannot be resized.
The buffer class is global. It deals with binary data directly and can be constructed in a variety of ways.

[Using Buffers in Node.js](https://www.w3resource.com/node.js/nodejs-buffer.php)

### 21. What’s the difference between `Buffer.alloc` and `Buffer.allocUnsafe`?
`Buffer.alloc` allocates a memory chunk, initializes it (sets every cell to either zero or some predefined value) and returns a Node.js Buffer wrapping this memory chunk.

`Buffer.allocUnsafe` skips the initialization stage. Instead it returns a Buffer pointing to uninitialized memory. This reduces the allocation time duration, but creates a possibility for(sensitive) data leakage, if this uninitialized memory is exposed to the user.
Thus, you should only `Buffer.allocUnsafe` only if you plan to initialize the memory chunk yourself.

### 22. How is the `slice` method on buffers different from that on arrays?
The `buf.slice()` method on buffers is a mutating operation which modifies the memory in the original buffer.
The `Array.prototype.slice()` method returns a shallow copy of a portion of an array and does not modify it.

### 23. What is the string_decoder module useful for? How is it different than casting buffers to strings?
The `string_decoder` module provides an API for decoding `Buffer` objects into strings in a manner that preserves encoded multi-byte UTF-8 and UTF-16 characters. It can be accessed using:
```
const { StringDecoder } = require('string_decoder');
```
The following example shows the basic use of the `StringDecoder` class.
```
const { StringDecoder } = require('string_decoder');
const decoder = new StringDecoder('utf8');

const cent = Buffer.from([0xC2, 0xA2]);
console.log(decoder.write(cent));

const euro = Buffer.from([0xE2, 0x82, 0xAC]);
console.log(decoder.write(euro));
```
When a `Buffer` instance is written to the `StringDecoder` instance, an internal buffer is used to ensure that the decoder string does not contain any incomplete multibyte characters. These are held in the buffer until the next call to `stringDecoder.write` or until `stringDecoder.end()` is called.

In the following example, the three UTF-8 encoded bytes of the European Euro synbol(€) are written over three separate operations:
```
const { StringDecoder } = require('string_decoder');
const decoder = new StringDecoder('utf8');

decoder.write(Buffer.from([0xE2]));
decoder.write(Buffer.from([0x82]));
console.log(decoder.end(Buffer.from([0xAC])));
```
[String Decoder](https://nodejs.org/api/string_decoder.html)

### 24. What are the 5 major steps that the require function does?
1. Check `Module._cache` for the cached module
2. If cache is empty, create a new Module instance and save it to the cache
3. Call `module.load()` with the given filename. This will call `module.compile()` after reading the file contents.
4. If there was an error loading/parsing the file, delete the bad module from the cache
5. return `module.exports`

[The Node.js Way - How `require()` Actually Works](http://fredkschott.com/post/2014/06/require-and-the-module-system/)

### 25. What is the `require.resolve` function and what is it useful for?
If you only want to resolve the module and not execute it, you can use the `require.resolve` function. This behaves exactly the same as the main `require` function, but does not load the file. It will still throw an error if the file does not exist and it will return the full path to the file when found.
This can be used for example, to check whether an optional package is installed or not and only use it when it's available.

[Requiring modules in Node.js](https://medium.freecodecamp.org/requiring-modules-in-node-js-everything-you-need-to-know-e7fbd119be8)

### 26. What is the `main` property in `package.json` useful for?
###### package.json:
The `package.json` file is core to the Node.js ecosystem and is a basic part of understanding and working with Node.js, npm and even modern JavaScript. The `package.json` is used as what equates to a manifest about applications, modules, packages, and more - it's a tool to that's used to make modern development streamlined, modular and efficient.

###### the `main` property:
The `main` property of a `package.json` is a direction to the entry point to the module that the `package.json` is describing. In a Node.js application, when the module is called via a require statement, the module's exports from the file named in the `main` property will be what's returned to the Node.js application.
Inside your `package.json`, the `main` property, with an entry point of `app.js`, would look loke this:
```
"main": "app.js",
```
[The Basics of Package.json in Node.js and npm](https://nodesource.com/blog/the-basics-of-package-json-in-node-js-and-npm/)

### 27. What are circular modular dependencies in Node and how can they be avoided?
###### Circular dependencies:
Circular dependencies(also known as cyclic dependencies) occur when two or more modules reference each other.

This could be a direct reference(A -> B -> A):
```
// file a.js
import { b } from 'b';
...
export a;

// file b.js
import { a } from 'a';
...
export b;
```
or indirect reference(A -> B -> C -> A):
```
// file a.js
import { b } from 'b';
...
export a;

// file b.js
import { c } from 'c';
...
export b;

// file c.js
import { a } from 'a';
...
export c;
```
While circular dependencies may not directly result in bugs(they certainly can), they will almost always have unintended consequences.

> In [Node.js docs](https://nodejs.org/api/modules.html#modules_cycles), it says, "Careful planning is required to allow cyclic module dependencies to work correctly within an application."

Circular dependencies are usually an indication of bad code design, and they should be refactored and removed if at all possible.

[Eliminate Circular Dependencies from Your JavaScript Project](https://spin.atomicobject.com/2018/06/25/circular-dependencies-javascript/)

**Two ways to avoid circular modular:**
1. move the `require` statements from the top of the file to the point in code they're actually used. This will delay their execution, allowing for the exports to have been created property.

2. restructure the code. For example move the code that both modules depend on into a new module C and let both A and B depend on C.

### IMPORTANT: What is `require`?
Node.js follows the CommonJS module system, and the buildin require function is the easiest way to include modules that exist in separate files.
The basic functionality is that it reads a JavaScript file, executes the file, and then proceeds to return the exports object.

### 28. What are the 3 file extensions that will be automatically tried by the require function?

`.js`, `.json` and `.node`.

### 29. When creating an http server and writing a response for a request, why is the end() function required?
Since the `res` object is a stream, we can write into it in several stages. the `end()` method indicates that we've finished writing into it and that the response is ready to be sent to the client.

### 30. When is it ok to use the file system *Sync methods(like readFileSync)?
###### Blocking:
**Blocking** is when the execution of additional JavaScript in the Node.js process must wait until a non-JavaScript operation completes. This happens because the event loop is unable to continue running JavaScript while a **blocking** operation is occurring.
Some methods also have blocking counterparts, which have names that end with `Sync`.

[Overview of Blocking vs Non-Blocking](https://nodejs.org/ja/docs/guides/blocking-vs-non-blocking/)

Every `fs` method in Node has a synchronous version. Why would you use a sync method instead of the async one?
Sometimes using the sync method is fine. For example, it can be used in any initializing step while the server is still loading. It is often the case that everything you do after the initializing step depends on the data you obtain there. Instead of introducing a callback-level, using synchronous methods is acceptable as long as what you use the synchronous methods for is a one-time thing.
However, if you are using synchronous methods inside a handler like an HTTP server on-request callback, that would simply be 100% wrong. DO NOT DO THAT.

When it is OK to block the process while the synchronous operation takes place. For example, this may be valid when writing a CLI tool. It's most likely not OK when writing a server that should handle multiple clients concurrently.

### 31. How can you print only one level of a deeply nested object?
You can use the `util.inspect` method.
```
const obj = {
    a: "a",
    b: {
        c: "c",
        d: {
            e: "e",
            f: {
                g: "g",
            }
        }
    }
};

const util = require('util');
console.log(util.inspect(obj, {depth: 0})); // prints: '{ a: \'a\', b: [Object]}'
console.log(util.inspect(obj, {depth: null})); // prints: '{ a: \'a\', b: { c: \'c\', d: { e: \'e\', f: { g: \'g\' } } } }'
```

### 32. What is the `node-gyp` package used for?
`node-gyp` is a cross-platform command-line tool written in Node.js for compiling native addon modules for Node.js. It bundles the gyp project used by the Chromium team and takes away the pain of dealing with the various differences in build platforms.

`node-gyp` is a tool which compiles Node.js Addons. Node.js Addons are native Node.js Modules, written in C or C++, which therefore need to be compiled on your machine. After they are compiled with tools like node-gyp, their functionality can be accessed via `require()`, just as any other Node.js Module.
If you do what you suggested the module won't work, you will need to compile it/build it with node-gyp on the system you moved the program to.

[node-gyp](https://github.com/nodejs/node-gyp)
[Node.js Addons](https://nodejs.org/api/addons.html)

### 33. The objects `exports`, `require`, and `module` are all globally available in every module but they are different in every module. How?
###### 1. Require:
`require` are used to consume modules. It allows you to include modules in your programs. You can add build-in core Node.js modules, community-based modules`(node_modules)` and local modules.
```
const fs = require('fs');

fs.readFile('./file.txt', 'utf-8', (err, data) => {
    if(err){ throw err; }
    console.log('data: ', data);
});
```
The `require` function will look for files in the following order:
1. Built-in core Node.js modules(like `fs`)
2. Modules in the `node_modules` folder.
3. If the module name has a `./`, `/` or `../`, it will look for the directory/file in the given path. It matches the file extensions: `*.js`, `*.json` and `*.node`.

###### 2. Exports:
The `exports` keyword gives you the chance to "export" your own objects and methods.

###### 3. Module Wrapper
You can think of it as a self-contained function
```
(function(exports, require, module, __filename, __dirname){
    module.exports = exports = {};

    // You module code ...
});
```
while `exports`, `require` and `module` appear to be global variables, they're actually specific to the module.

### 34. How can a module be both requirable by other modules and executable directly using the node command?
###### Module:
A module is a discrete program, contained in a single file in Node.js. Modules are therefore tied to files, with one module per file. Modules are available in other programming languages. Node.js uses the CommonJS system of modules, but there are other module types used in the JavaScript ecosystem. The most prominent of these other module systems are the Asynchronous Module Definition(AMD) and the (ECMAScript 6) ES6 module systems.

###### module.exports:
`module.exports` is an object that the current module returns when it is "required" in another program or module.

[How to use module.exports in Node.js](https://stackabuse.com/how-to-use-module-exports-in-node-js/)

A module can detect it its being requirable or executed directly by inspecting the `require.main` value:
```
if(require.main == module){
    console.log('called directly.');
}else{
    console.log('required as a module.');
}
```
###### Accessing the main module:
When a flie is run directly from Node.js, `require.main` is set to its `module`. That means that it is possible to determine whether a file has been run directly by testing `require.main === module`.
For a file `foo.js`, this will be `true` if run via `node foo.js`, but `false` if run by `require('./foo')`.
Because `module` provides a `filename` property(normally equivalent to `__filename`), the entry point of the current application can be obtained by checking `require.main.filename`.

### 35. What’s an example of a built-in stream in Node that is both readable and writable?
###### Streams:
Streams are collections of data - just like arrays or strings. The difference is that streams might not be available all at once and they don't have to fit in memory. This makes streams really powerful when working with large amounts of data, or data that's coming from an external source one **chuck** at a time.

[Node’s Streams](https://jscomplete.com/learn/node-beyond-basics/node-streams)

###### duplex stream:
A duplex stream is a stream that implements both a readable and a writable. These streams allow data to pass through. Readable streams will pipe data inso a duplex stream, and the duplex stream can also write that data. So duplex stream represent the middle sections of pipelines.

`net.Socket` is an example for a Duplex(both readable and writable) stream.

### 36. What’s the difference between using event emitters and using simple callback functions to allow for asynchronous handling of code?
The biggest difference between using callbacks and using event emitters is that callbacks directly couples the original function and the callback, whereas using event emitters can let you keep the calling function and the callback separate.

An event handler is a callback to an event that occured like a mouse click. More specifically, an event handler is the callback method that is passed into a event listener. When the listener is triggered, it fires its callback or in this case, the event handler.

Callbacks methods do not need to be associated with events. In fact, generally speaking, when someone uses the term callback, they are referring to a function that gets passed as a parameter into a method which is fired when the method completes.

### 37. The `require` function always caches the module it requires. What can you do if you need to execute the code in a required module many times?
The cache in which modules are cached in is accessible using `require.cache`. Thus, if you delete a module key from `require.cache`, the next time you require it will reload it(and will execute the code in it again).

### 38. What’s the difference between the Paused and the Flowing modes of readable streams?
Readable streams operate in either *paused* or *flowing* modes. When is *flowing* mode, data is read from the underlying system automatically and provided to an application as quickly as possible using events via the EventEmitter interface. In *paused* mode, the `stream.read()` method must be called explicitly to read chunks of data from the stream.

### 39. What does the `--inspect` argument do for the node command?
V8 Inspector integration allows attaching Chrome DevTools to Node.js instances for debugging and profiling. It uses the [Chrome DevTool Protocol](https://chromedevtools.github.io/devtools-protocol/)

V8 Inspector can be enabled by passing the `--inspect` flag when starting a Node.js application. It is also possible to supply a custom port with that flag *e.g.* `--inspect=9222` will accept DevTools connections on port 9222.

### 40. When working with streams, when do you use the pipe function and when do you use events? Can those two methods be combined?
###### Pipes in Node.js:
Within Node applications, streams can be piped together using the pipe() method, which takes two arguments:
- A required writable stream that acts as the destination for the data.
- An optional object used to pass in options.

see [code example](https://www.guru99.com/node-js-streams-filestream-pipes.html#9)

###### Events in Node.js:
Events are one of the key concepts in Node.js and sometimes Node.js is referred to as an Event-Driven framework.
Basically, an event is something that happens. For example, if a connection is established to a database, then the database connection event is triggered. Event driven programming is to create function that will be triggered when specific events are triggered.

see [code example](https://www.guru99.com/node-js-streams-filestream-pipes.html#10)

`.pipe()` is a function that takes a readable source stream `src` and hooks the output to a destination writable stream `dst`
```
    src.pipe(dst)
```
essentialy it means that `.pipe()` takes care of listening for `data` and events from `src`. So, to answer the questions, using `.pipe()` can make the code more straight forward when this is the functionaly you're interested in.
Events can be used to tailor more specific functionaly for your use case.