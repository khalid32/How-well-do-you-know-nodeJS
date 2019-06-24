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

## Working with HTTPS
HTTPS is the HTTP protocol over TLS/SSL. Node has a separate module to work with HTTPS, but it's very similar to the HTTP module. To convert the basic HTTP server example to work with HTTPS, all we need to do is require https instead and provide the createServer method with an `options object`. This object can be used to configure multiple things. 

```javascript
const fs = require('fs');
const server = require('https')
  .createServer({
    key: fs.readFileSync('./key.pem'),
    cert: fs.readFileSync('./cert.pem'),
  });

server.on('request', (req, res) => {
  res.writeHead(200, { 'content-type': 'text/plain' });
  res.end('Hello world\n');
});

server.listen(443);
```
We first need to generate a certificate. We can use the `openSSL toolkit` for that, which will allow us to first generate a private key. We can have it encrypted or not encrypted, and it will allow us to generate a certificate signing request(CSR), and then self-sign this certificate to test it. Of course, the browsers will not trust out self-signed certificate, but it's good for testing purposes.

We can actually combine all these steps with one command that will output key and a certificate file for us.
```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -nodes
```

## Requesting HTTP/HTTPS Data
Node can also be used as a client for requesting http and https data.
There are 5 major classes of objects in Node's HTTP module.
- `http.Server` is what we use to create basic server, it inherits from `net.Server(net.Server/EE)`, so it's an EventEmitter.
- `http.Agent(globalAgent/new Agent())` class is used to manage pooling sockets used in HTTP client requests. Node uses a global agent by default, but we can create a different agent with different options when we need to.
- `http.IncomingMessage`
- `http.ServerResponse` object gets created internally by an HTTP server
- `http.ClientRequest` is different from the request object.

Both `clientRequest` and `ServerResponse` implement the `writable stream interface(WritableStream/EE)`.
`IncomingMessage` objects implement the `readable stream interface(ReadableStream/EE)`, and rest of the objects are event emitters.

```javascript
// ~~~ request.js ~~~
const https = require('https');

// req: http.ClientRequest
const req = https.get(
  'https://www.google.com',
  (res) => {
    // res: http.IncomingMessage
    console.log(res.statusCode);
    console.log(res.headers);

    res.on('data', (data) => {
      console.log(data.toString());
    });
  }
);

req.on('error', (e) => console.log(e));

console.log(req.agent); // http.Agent

// ~~~ server.js ~~~
const server = require('http').createServer();

server.on('request', (req, res) => {
  // req: http.IncomingMessage
  // res: http.ServerResponse

  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello world\n');
});

server.listen(8000);
```

<details><summary><b>5 major classes of objects and where to identify them:</b></summary>
<p>
In the request example, the request object here is from the class clientRequest.
The response object is of type incomingMessage. And the agent that was used for the request is of type http Agent.

In the server example, the server object is from the http server class, the request object inside the request listener is from the IncomingMessage class, and the response object is from the server response class.
</p>
</details>