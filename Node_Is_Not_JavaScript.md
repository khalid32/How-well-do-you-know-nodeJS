# Node != JavaScript

## Node's Architecture: V8 and libuv

Node uses a VM, which is V8 by default, to execute JavaScript code, which means the JavaScript features that are available in Node are really the JavaScript features supported by the V8 engine shipped with Node.

This support is managed with 3 feature groups
- `Shipping`
- `Staged` and
- `In progress`

![V8 Feature Groups](https://user-images.githubusercontent.com/8571179/57565680-f06daa00-73e3-11e9-9020-6d7281b77f18.png)


`Shipping` features are on by default, `Staged` and `In progress` features are not, but we can use command-line flags to enable them.

`Staged` features are almost complete but not quite there yet. But we can use the `--harmony` flag to enable them.

`In Progress` features are less stable, but we can still enable them if we want with specific flags. You can see a list of all the `in progress` features available in the currently used node with a command below.
```bash
node --v8-options | grep "in progress"
```
**Output:**
```bash
  --harmony-do-expressions (enable "harmony do-expressions" (in progress))
  --harmony-class-fields (enable "harmony fields in class literals" (in progress))
  --harmony-static-fields (enable "harmony static fields in class literals" (in progress))
  --harmony-array-flatten (enable "harmony Array.prototype.flat{ten,Map}" (in progress))
  --harmony-locale (enable "Intl.Locale" (in progress))
```

You can actually set them at run time using the v8 module.
```bash
node
> v8
```
**Output:**
```bash
{ cachedDataVersionTag: [Function: cachedDataVersionTag],
getHeapStatistics: [Function: getHeapStatistics],
getHeapSpaceStatistics: [Function: getHeapSpaceStatistics],
setFlagsFromString: [Function: setFlagsFromString],
Serializer: [Function: Serializer],
Deserializer: [Function: Deserializer],
DefaultSerializer: [Function: DefaultSerializer],
DefaultDeserializer: [Function: DefaultDeserializer],
deserialize: [Function: deserialize],
serialize: [Function: serialize] }
```

The more useful methods on the V8 module are the `getHeapStatistics` method, which can be used to get information about heap memory, like its total size, the currently used size, and old/new heap spaces and more
```bash
> v8.getHeapStatistics()
```
**Output:**
```bash
{ total_heap_size: 9682944,
  total_heap_size_executable: 1048576,
  total_physical_size: 7054096,
  total_available_size: 1518088656,
  used_heap_size: 5867496,
  heap_size_limit: 1526909922,
  malloced_memory: 8192,
  peak_malloced_memory: 702824,
  does_zap_garbage: 0 }
```

![V8 Architecture](https://user-images.githubusercontent.com/8571179/57565652-5443a300-73e3-11e9-8d69-5932f3a07627.png)

Node is more than a wrapper for V8, it provides APIs for working with operating system files, binary data, networking and much more. It's useful to understand how V8 and Node interact and work together. 
- 1st, Node uses V8 via V8's C++ API. Node itself has an API which we can use in JavaScript, and it allows us to interact with the filesystem, network, timers and others.
- The Node API eventually executes C++ code using V* object and function templetes, but it's not part of V8 itself.
- Node also handles the waiting for asynchronous events for us using libuv.
- When Node is done waiting for I/O operations or timers, it usually has callback functions to invoke, and it's time to invoke these callbacks, Node simply passes the control into the V8 engine.
- When V8 is done with the code in the callback, the control is passed back to Node. This is important to understand as when the control is with V8 and since V8 is single-threaded, Node cannot execute anymore JavaScript code, no matter how many callbacks have been registered, Node will wait until V8 can handle more operations. This is actually what makes programming in Node easy.

libuv is used to abstract the non-blocking I/O operations to a consistent interface across many operating systems. It's what handles operations on the file system, TCP/UDP sockets, child processes, and others.
libuv includes a thread pool to handle what can't be done asynchronously at the operating system level.
libuv is also what provides Node with the event-loop.

Other than V8 and libuv, Node has a few more dependencies
- `http-parser` is a small C library for parsing HTTP messages. It works for both requests and responses and it's designed to have a very small pre-request memory footprint.
- `C-ares` is what enables performing asynchronous DNS queries.
- `OpenSSL` is used mostly in the tls and crypto modules. It provides implementations for many cryptographic functions.
- `Zlib` is used for its fast async and streaming compression and decompression interfaces.

## Node's CLI and REPL
Running the Node command without arguments starts a **REPL(Read, Eval, Print, Loop)**.

One of the most useful features of Node's REPL is the `auto-complete`. If you just Tab-Tab on an empty line, you get this big list.
Auto-complete works on any object. For example....
```bash
## for array
> var arr = [];
undefined
> arr.
arr.__defineGetter__      arr.__defineSetter__      arr.__lookupGetter__      arr.__lookupSetter__      arr.__proto__             arr.hasOwnProperty        arr.isPrototypeOf         arr.propertyIsEnumerable  arr.valueOf

arr.concat                arr.constructor           arr.copyWithin            arr.entries               arr.every                 arr.fill                  arr.filter                arr.find                  arr.findIndex
arr.forEach               arr.includes              arr.indexOf               arr.join                  arr.keys                  arr.lastIndexOf           arr.map                   arr.pop                   arr.push
arr.reduce                arr.reduceRight           arr.reverse               arr.shift                 arr.slice                 arr.some                  arr.sort                  arr.splice                arr.toLocaleString
arr.toString              arr.unshift               arr.values                
arr.length

## for object
> var obj = {};
undefined
> obj.
obj.__defineGetter__      obj.__defineSetter__      obj.__lookupGetter__      obj.__lookupSetter__      obj.__proto__             obj.constructor           obj.hasOwnProperty        obj.isPrototypeOf         obj.propertyIsEnumerable
obj.toLocaleString        obj.toString              obj.valueOf 
```

> Node's REPL remembers the lines you previously tested and you can navigate to them with up/down arrow.

Another helpful REPL feature is the underscore(`_`). You can use it to access the last evaluated value.

Node's REPL has special commands that all begin with a `.`, so `. + Tab` will get you a list of those and `.help` will give you a description of each.
```bash
> .
break   clear   editor  exit    help    load    save

> .help
.break    Sometimes you get stuck, this gets you out
.clear    Alias for .break
.editor   Enter editor mode
.exit     Exit the repl
.help     Print this help message
.load     Load JS from a file into the REPL session
.save     Save all evaluated commands in this REPL session to a file
```

Node has a build-in REPL module, which is what it uses to give us the default REPL mode, but we can use this module to create REPL sessions, we just require it and invoke the start function.

## Global Object, Process, and Buffer
> You're not a node expert unless you recognize and know how to use everything you see in the global list of objects in REPL mode.

Two of the most important things that are available on the `global object` in a normal process are the `process object` and `buffer object`.
The node process object provides a bridge between a `Node application` and `its running environment`. It has many useful properties.
```bash
node -p "process" | less
```
We can use `process.versions` to read versions of the current node and its dependencies.
```bash
node -p "process.versions"

## output
{ http_parser: '2.8.0',
  node: '10.10.0',
  v8: '6.8.275.30-node.24',
  uv: '1.23.0',
  zlib: '1.2.11',
  ares: '1.14.0',
  modules: '64',
  nghttp2: '1.33.0',
  napi: '3',
  openssl: '1.1.0i',
  icu: '62.1',
  unicode: '11.0',
  cldr: '33.1',
  tz: '2018e' }
```
One of the most useful properties on the process object is the env property.
```bash
node -p "process.env" | less
```
The env property exposes a copy of the user environment(which is the list of strings you get with the ENV command in linux machine and the SET command in Window).
If we modify `process.env`, which we can actually do, we won't be modifying the actual user environment, so keep that in mind.
> You should actually not read from `process.env` directly. We actually use configuration variables like passwords or API keys from the environment. Also which ports to listen to, which database URIs to connect to. You should put all of these behind a configuration or settings module and always read from that module, not `process.env` directly.

```bash
node -p "process.release.lts"
```
This will have the LTS label of the used node release, and it will be undefined if the currently used Node release is not LTS, so we can check this label and maybe show a warning if an application is being started in production on a non LTS node.

The most useful thing we can do with the process object is to communicate with the environment, and for that we use the standard streams
- `stdin` for read
- `stdout` for write
- `stderr` to write any errors

Those are pre-established ready streams and we can't actually close them.

The process object is an instance of EventEmitter. This means we can emit events from process and we listen to certain events on the process.


The Buffer class, also available on the global object, is used heavily in Node to work with binary streams of data. 
```bash
> Buffer

## Output
{ [Function: Buffer]
  poolSize: 8192,
  from: [Function: from],
  of: [Function: of],
  alloc: [Function: alloc],
  allocUnsafe: [Function: allocUnsafe],
  allocUnsafeSlow: [Function: allocUnsafeSlow],
  isBuffer: [Function: isBuffer],
  compare: [Function: compare],
  isEncoding: [Function: isEncoding],
  concat: [Function: concat],
  byteLength: [Function: byteLength],
  [Symbol(kIsEncodingSymbol)]: [Function: isEncoding] }
```
A buffer is essentially a chunk of memory allocated outside of the V8 heap, and we can put some data in that memory, and that data can be interpreted in one of many ways, depending on the length of a character. That's why when there is a buffer, there is a character encoding, because whatever we place in a Buffer does not have any character encoding, so to read it, we need to specify an encoding.

When we read content from files or sockets, if we don't specify an encoding, we get back a buffer object.
So a buffer is lower-level data structure to represent a sequence of binary data, and unlike arrays, **once a buffer is allocated, it cannot be resized**.

We can create a buffer in one of [3 major ways](https://nodejs.org/api/buffer.html#buffer_buffer_from_buffer_alloc_and_buffer_allocunsafe):
- `Buffer.from()`
- `Buffer.alloc()` and
- `Buffer.allocUnsafe()`

`Buffer.alloc()` creates a filled buffer of certain size, while `Buffer.allocUnsafe()` will not fill the created buffer
```bash
> Buffer.alloc(8)
<Buffer 00 00 00 00 00 00 00 00>
> Buffer.allocUnsafe(8)
<Buffer 48 42 ee 80 86 7f 00 00>
```
so that might contain old or sensitive data, and need to be filled right away. To fill a buffer we can use `buffer.fill()`.
```bash
> Buffer.allocUnsafe(8).fill()
<Buffer 00 00 00 00 00 00 00 00>
```
We can also create a buffer using the `from` method(`Buffer.from`), which accepts a few different types in its argument.
Buffers are useful when we need to read things like an image file from a TCP stream or a compressed file, or any other form of binary data access.

One final note on buffers, when converting streams of binary data, we should use the `string_decoder` module, because it handles multi-byte characters much better, especially incomplete multibyte characters. 
The string decoder preserves the incomplete encoded characters internally until it's complete and then returns the result.

## How require() Actually Works
Modularity is a first concept in Node, and fully understanding how it works is a must.
There are 2 core modules involved, the `require` function, which is available on the global object, but each module gets its own require function.
And Module module, also available on the global object, and is used to manage all the modules we require with the require function.

Requiring a module in node is a very simple concept, to execute a require call, Node goes through the following sequence of steps:
- `Resolving`, to find the absolute file path of a module.
- `Loading`, is determined by the content of the file at the resolved path.
- `Wrapping`, is what every module it's private scope, and what makes require local to every module.
- `Evaluating`, is what the VM eventually does with the code.
- And then `Caching`, so that when we require this module again, we don't go over all the steps again.

#### Module Properties
```bash
node -p "module"

## output
Module {
  id: '[eval]',
  exports: {},
  parent: undefined,
  filename: '/home/k32/[eval]',
  loaded: false,
  children: [],
  paths: 
   [ '/home/k32/node_modules',
     '/home/node_modules',
     '/node_modules' ] }
```
First, an id to identify it.
The path to the filename can be accessed with the filename property.

Node Modules have a `one-to-one` relation with files on the file-system. However, before we can load the content of a file into the memory, we need to find the location of a file. 

If we want to resolve the module, not to execute it, we can use `require.resolve` method. This behaves exactly the same as require, but does not load the file.
```javascript
// in index.js

require.resolve('app'); // resolving app.js file
```
It will still throw an error if the file does not exist. This can be used, for example, to check whether an optional package is installed or not.

## JSON and C++ Addons
We can define and require JSON files, and C++ Addons files with the node require function.
The 1st thing Node will try to resolve is a `.js` file. If it can't find a `.js` file, it will try a `.json` file and it will parse the file if found as a JSON text file. After that, it will try to find a binary `.node` file.
```javascript
  // 1. try something.js
  // 2. try something.json
  // 3. try something.node
```
[C++ Addons](https://nodejs.org/api/addons.html)

## Wrapping and Caching Modules
Node's wrqpping of modules is often misunderstood.
We can use the export object to export properties, but we cannot replace the export object directly. When we need to replace the export object, we need to use the `module.exports` syntax. **The question is....WHY?**
```javascript
  exports.id = 1; // this is ok
  exports = { id: 1 }; // this is not ok
  module.exports = { id: 1 }; // this is ok
  // WHY???
```
Also, we've seen how the variables we define in the module scope here will not be available outside the module. Only the things that we export are available. So how caome the variables are magically `scoped` and the exports object can't be replaced directly?
The answer is simple...
Before compilling a module, Node wraps the module code in a function, which we can inspect using the wrapper property of the `module` module. This dunction has 5 arguments:
- `exports`
- `require`
- `module`
- `__filename` and
- `__dirname`
```bash
> require('module').wrapper
[ '(function (exports, require, module, __filename, __dirname) { ',
  '\n});' ]
```
This function wrapping process is what keeps the top-level variables in any module scoped to that module and it is what makes the module/exports/require variables appear to look global when, in fact, they are specific to each module. Same thing for the __filename/__dirname variables, which will contain the module's absolute filename and directory path.

The wrapping functions return value is this exports object reference. Note how we have both exports and the module object itself passed in the wrapper function.
Exports is simply a variable reference to `module.exports`.
There is nothing special about require. It's a function that takes a module name or path and returns the exports object.

Because of module's caching, Node caches the first call and does not load the file on the second call. We can see the cache using `require.cache`, and in there you will find an entry for the `.js` file.
