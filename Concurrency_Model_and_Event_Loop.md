# Concurrency Model and Event Loop

## What Is I/O Anyway?
I/O is used to label a communication between a process in a computer CPU and anything external to that CPU, including memory, disk, network, and even another process.

![I/O work procedure](https://user-images.githubusercontent.com/8571179/58102337-f1cc7d00-7c02-11e9-92f6-c53538126516.png)

The process communicates with these external things with signals or messages. Those signals are input when they are received by the process, and output when they are sent out by the process.

The term I/O is really overused, because naturally, almost every operation that happens inside and outside computers is an I/O operation, but when talking about Node's architecture, the term I/O is usually used to reference accessing disk and network resources, which is the most time-expensive part of all operations.

Node's event loop is designed around the major fact that the largest waste in computer programming comes from waiting on such I/O operations to complete.

![Handling Slow I/O](https://user-images.githubusercontent.com/8571179/58102390-03158980-7c03-11e9-9b7a-a2d8b99b0eed.png)

We can handle requests for these slow operations in one of many ways.
- We can just execute things synchronously. This is the easiest way to go about it, but it's horrible because one request is going to hold up other requests.
- We can fork a new process from the OS to handle each request, but that's probably won't scale very well with a lot of requests.
- The most popular method for handling these requests is `threads`. We can start a new thread to handle each request. But threaded programming can get very complicated when threads start accessing shared resources.
- Single threaded frameworks like Node use an event loop to handle requests for slow I/O operations without blocking the main execution runtime.

## The Event Loop
The simplest one-line definition of the event loop is this:
> **The `entity` that handles `external events` and converts them into `callback invocations`.**

Another Simplest definition:
> **A loop that picks events from the event queue and pushes their callbacks to the call stack.**

![Event Loop working process](https://user-images.githubusercontent.com/8571179/58104053-e2026800-7c05-11e9-81c2-8c9954b904ca.png)

What I want you to understand first is that there is this thing called the event loop that Node automatically starts when it executes a script, so there is no need for us to manually start it.
This event loop is what makes the *asynchronous callback programming style* possible.
Node will actually exit the event loop when there are no more callbacks to perform.
The event loop is also present in browsers and it's very similar to the one that fires in Node.

## The Call Stack
The V8 Call Stack which is simply a list of functions.
A stack is a `first in last out(FILO)` simple data structure. The top element that we can pop out of the stack is the last element that we pushed into it.

When running a code, V8 uses the stack to record where in the program it is currently executing. Every time we step into a function, it gets pushed to the stack, and every time we return from a function, it gets popped out of the stack.

## Handling Slow Operations
As long as the operations we execute in the call stack are fast, there is no problem with having a single thread, but when we start dealing will slow operations, the fact that we have a single thread becomes a problem, because these slow operations will block the executions.

## How Callbacks Actually Work
We all know that Node API is designed around callbacks. We pass functions to other functions as arguments, and those argument functions get executed at a later time, somehow.

It's important to understand that an API call like `setTimeout` is not part of V8. It's provided by Node itself, just like it's provided by browsers too. It's wired in a way to work with the event loop asynchronously. That's why it behaves a bit weirdly on the normal call stack. 

Queue is simply a list of things to be processed.

## `setImmediate` & `process.nextTick`
Because of a loop, the timers is not really executed after 0 milliseconds, but rather after we're done with the stack, so if there was a slow operation on the stack, those timers will have to wait.
The delay we define in a timer is not a guaranteed time to execution, but rather a minimum time to execution. The timer will execute after a minimum of this delay. 


Node's event loop has multiple phases. The timers run in one of those phases while most I/O operations run in another phase.
Node has a special timer...
- `setImmediate`, which runs in a seperate phase of the event loop.</br>
It's mostly equivalent to a 0ms timer, except in some situations, `setImmediate` will actually take precedence over previously defined 0ms `setTimeouts`. It's generally recommended to always use `setImmediate` when you want something to get executed on the next tick of the event loop.

Ironically, Node has a `process.nextTick` api that is very similar to `setImmediate`, but Node actually does not execute its callback on the next tick of the event loop, so the name here is misleading, but it's unlikely to change.

`process.nextTick` is not technically part of the event loop, and it does not care about the phases of the event loop. Node processes the callbacks registered with nextTick after the current operation completes and before the event loop continues. This is both useful and dangerous, so be careful about it, especially when using `process.nextTick` recursively.