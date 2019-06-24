# Node for Web

## The Basic Streaming HTTP Server
HTTP is a first class citizen in Node. In fact, node started as a web server and evolved into the much more generalized framework it is today.
Node's HTTP module is designed with streaming and low latency in mind.

```javascript
const server = require('http').createServer();

server.on('request', (req, res) => {
  res.writeHead(200, { 'content-type': 'text/plain' });
  res.write('Hello world\n');

  setTimeout(function () {
    res.write('Another Hello world\n');
  }, 10000);

  setTimeout(function () {
    res.write('Yet Another Hello world\n');
  }, 20000);
});

server.listen(8000);
```

`Keep-alive` means that the connection to the web server will be persisted.

The TCP connection will not be killed after a requested receives a response, so that they can send multiple requests on the same connection.
`Transfer-encoding chunked` is used to send a variable length response text. It basically means that the response is being streamed! Node is ok with us sending partial chunked responses, because the response object is a writable stream. There is no response length value being sent.

<details><summary><b>Note:</b></summary>
<p>
However, that terminating the response object with a call to the end method is not optional, you have to do it every request. If you don't, the request will timeout after the default timeout period, which is set to two minutes.
</p>
</details>

