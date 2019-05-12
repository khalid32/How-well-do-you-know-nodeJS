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
Two of the most important things that are available on the `global object` in a normal process are the `process object` and `buffer object`.
The node process object provides a bridge between a Node application and its running environment. It has many useful properties.
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
