# Node's Event Driven Architecture

## Node's Common Built-in Modules
The original way Node handled asynchronous calls is with `callbacks`.

> It is important to understand here that callbacks do not indicate an asynchronous call in the code.

Callbacks !== Asynchrony

Adding a promise interface makes your code a lot easier to work with when there is a need to loop over an async function. With callbacks, things become messy. Promises improve that a little bit, and function generators improve on that a little bit more. But a more recent alternative to working with async code is to use the async function, which allows us to treat async code as if it was linear, making it a lot more readable when we need to process things in loops.

## Event Emitter
The EventEmitter is a module that facilitates communication between objects in Node.
Event Emitter is at the core of Node asynchronous event-driven architecture.

The concept is simple: emitter objects emit named events that cause listeners to be called.
```javascript
// Import
const EventEmitter = require('events');

// Extend
class Logger extends EventEitter {};

// Init
const logger = new Logger();

// Emit
logger.emit('event'); // emitting named events

// AddListener
logger.on('event', listenerFunc); // registering listener functions
```

So an emitter object has 2 main features,
- `emitting named events` and
- `registering listener functions`

To work with EventEmitter, we just create a class that extends EventEmitter.
Emitter objects are what we instentiate from EventEmitter-based classes.
At any point in their lifecycle, we can use the emit function to emit any named event we want.
Emitting an event is the signal that some condition has occurred. This condition is usually about a state change in the emitting object.
We can add listener functions using the on method, and those listener functions will simply be executed every time the emitter object emits their associated named event.

Emitting an event is the signal that some condition has occured. This condition is usually a state change in the emitting object.

Events !== Synchronous/Asynchronous code.

One benefit for using events instead of regular callbacks is that we can react to the same signal multiple times by defining multiple listeners. To accomplish the same with callbacks, we have to write more logic for this inside the single available callback.

Events are a great way for applications to allow multiple external plugins to build functionality on top of the application's core. You can think of them as hook points to allow for customizing the story around a state change.

