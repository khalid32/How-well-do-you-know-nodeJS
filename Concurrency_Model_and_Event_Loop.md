# Concurrency Model and Event Loop

## What Is I/O Anyway?
I/O is used to label a communication between a process in a computer CPU and anything external to that CPU, including memory, disk, network, and even another process.

The process communicates with these external things with signals or messages. Those signals are input when they are received by the process, and output when they are sent out by the process.

The term I/O is really overused, because naturally, almost every operation that happens inside and outside computers is an I/O operation, but when talking about Node's architecture, the term I/O is usually used to reference accessing disk and network resources, which is the most time-expensive part of all operations.

Node's event loop is designed around the major fact that the largest waste in computer programming comes from waiting on such I/O operations to complete.

We can handle requests for these slow operations in one of many ways.
- We can just execute things synchronously. This is the easiest way to go about it, but it's horrible because one request is going to hold up other requests.
- We can fork a new process from the OS to handle each request, but that's probably won't scale very well with a lot of requests.
- The most popular method for handling these requests is `threads`. We can start a new thread to handle each request. But threaded programming can get very complicated when threads start accessing shared resources.
- Single threaded frameworks like Node use an event loop to handle requests for slow I/O operations without blocking the main execution runtime.