# Working with Streams

> "Streams are Node's best and most misunderstood idea" --- Dominic Tarr

## Stream All the Things
Streams, most importantly, give you the power of composability in your code. Just like you can compose powerful Linux commands by piping other smaller commands, you can do exactly the same in Node with Streams.

Many of the built-in modules in Node implement the streaming interface. This list has some examples for readable and writable streams. Some of these items are both readable and writable streams, and you can notice how the items are closely related, so while an HTTP response is a readable stream on the client, it's a writable stream on the server, because basically one object generates the data and the other object consumes it. Note also how `stdin/out` and error streams are the inverse type when it comes to child processes.

![Readable and Writable Streams](https://user-images.githubusercontent.com/8571179/60153006-a4948a00-9804-11e9-8991-a1cd89453e5b.png)


**What are Streams?**
Collections of data that might not be available all at once and don't have to fit in memory.

Streams are simply collections of data, just like arrays or strings, with the difference that they might not be available all at once and they don't have to fit in memory, which makes them really powerful for working with large amounts of data or data that's coming from an external source one chunk at a time.

## Streams 101

![Types of Streams](https://user-images.githubusercontent.com/8571179/60153760-1077f200-9807-11e9-8a59-2e2cca1f420c.png)

- A `Readable` stream is an abstraction for a source from which data can be consumed. An example of that is the `fs.createReadStream` method.
- A `Writable` stream is an abstraction for a destination to which data can be written. An example of that is the `fs.createWriteStream` method.
- `Duplex` streams are both Readable and Writable, like a `socket`.
- `Transform` streams are basically duplex streams that can be used to modify or transform the data as it is written and read. An example of that is the zlib `createGzip` stream to compress the data using `gzip`. </br>You can think of a transform stream as a function where the input is the writable stream part and the output is readable stream part. </br>You might also hear transform streams referred to as "*Through Streams*".

<span style="color: #d63031; background-color: #fab1a0;font-size: 2em;">All streams are *instances* of EventEmitter</span>
They all emit events that we can use to write or read data from them.
How ever, we can consume streams in a simpler way using the pipe method.
```javascript
src.pipe(dst);
```

In this simple line, we're piping the output of a readable stream, `src`, as the input of a writable stream, `destination`.
`src` has to be `readable stream` and `destination` has to be a `writable stream` here. They can both, of course, be duplex streams as well. In fact, if we're piping into a duplex stream, we can chain pipe calls just like we do in Linux.
```bash
## piping in Linux
a | b | c | d

## piping in Node.js
a.pipe(b).pipe(c).pipe(d)
##        ^
##        |
##        |
## Above line is equivalent to,
##      a.pipe(b);
##      b.pipe(c);
##      c.pipe(d);
```
It's generally recommanded to either use pipe or events, but avoid mixing them, and usually when you're using pipe you don't need to use events.

There are 2 main different things about them.
- `Implementing`
- `Consuming`

There is the task of implementing the streams and there is the consuming of the streams.

Stream implementers are usually who use the stream module.
For consuming, all we need to do is either use pipe or listen to the stream events.

![Readable and Writable Stream](https://user-images.githubusercontent.com/8571179/60564639-475b8400-9d82-11e9-8fa0-beb2f405b71a.png)

Readable and Writable streams have events and functions that are somehow related. We usually use them together. Some of events are similar, like the `error` and `close` events, and others are different.
The most important events on a readable stream are the data event, which is emitted whenever the stream passes a chunk of data to the consumer, and the end event, which is emitted when there is no more data to be consumed from the stream. The most important events on a writable stream are the drain event, which is a signal that the writable stream can receive more data, and the finish event, which is emitted when all data has been flushed to the underlying system. To consume a readable stream, we use either the pipe/unpipe methods, or the read/unshift/resume methods, and to consume a writable stream, we just write it with the write method and call the end method when we're done. 

![Readable Streams](https://user-images.githubusercontent.com/8571179/60564659-635f2580-9d82-11e9-88fb-abad271e659c.png)

Readable streams have 2 main modes that affect the way we consume them. They can be either in the paused mode or in the flowing mode. Those are sometimes referred to as pull vs. push modes. All readable streams start in the pause mode, but they can be easily switch into flowing and back to paused where needed.
In Paused mode, we have to use the `.read()` method to read from the stream, while in the flowing mode, the data is continuously flowing and we have to listen to events to consume it.
In the flowing mode, data can actually be lost if no consumers are available to handle it, this is why when we have a readable stream in flowing mode, we need a `'data'` event handler. In fact, just adding a data event handler switchers a paused stream into flowing mode, and removing the data handler switches it back to paused mode.

![resume and pause method](https://user-images.githubusercontent.com/8571179/60564686-75d95f00-9d82-11e9-837b-dbb324c549b0.png)

Some of this is done for backward compatibility with the older stream interface in node. Usually, to switch between these 2 modes, we use the resume and pause methods.