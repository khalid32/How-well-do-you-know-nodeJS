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

In this simple line, we're piping the output of a readable stream, src, as the input of a writable stream, destination.
`src` has to be readable stream and destination has to be a writable stream here. They can both, of course, be duplex streams as well. In fact, if we're piping into a duplex stream, we can chain pipe calls just like we do in Linux.
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
