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