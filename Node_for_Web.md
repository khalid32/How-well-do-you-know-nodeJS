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