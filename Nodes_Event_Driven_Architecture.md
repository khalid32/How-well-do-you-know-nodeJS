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
logger.emit('event');

// AddListener
logger.on('event', listenerFunc);
```

So an emitter object has 2 main features,
- `emitting named events` and
- `registering listener functions`


Emitting an event is the signal that some condition has occured. This condition is usually a state change in the emitting object.