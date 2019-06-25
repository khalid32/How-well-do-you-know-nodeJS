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

The HTTP module has a `globalAgent`, which node uses to manage the sockets here.
```bash
node
> http.globalAgent

Agent {
  domain:
   Domain {
     domain: null,
     _events:
      { removeListener: [Function: updateExceptionCapture],
        newListener: [Function: updateExceptionCapture],
        error: [Function: debugDomainError] },
     _eventsCount: 3,
     _maxListeners: undefined,
     members: [] },
  _events: { free: [Function] },
  _eventsCount: 1,
  _maxListeners: undefined,
  defaultPort: 80,
  protocol: 'http:',
  options: { path: null },
  requests: {},
  sockets: {},
  freeSockets: {},
  keepAliveMsecs: 1000,
  keepAlive: false,
  maxSockets: Infinity,
  maxFreeSockets: 256
}
```
It has some pre-configured options here. We can see that agent information here if we do a `request.agent`.

<details><summary><b>5 major classes of objects and where to identify them:</b></summary>
<p>
In the request example, the request object here is from the class clientRequest.
The response object is of type incomingMessage. And the agent that was used for the request is of type http Agent.

In the server example, the server object is from the http server class, the request object inside the request listener is from the IncomingMessage class, and the response object is from the server response class.
</p>
</details>

## Working with Routes
```javascript
const fs = require('fs');
const server = require('http').createServer();
const data = {};

server.on('request', (req, res) => {
  switch (req.url) {
  case '/api':
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify(data));
    break;
  case '/home':
  case '/about':
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end(fs.readFileSync(`.${req.url}.html`));
    break;
  case '/':
    res.writeHead(301, { 'Location': '/home' });
    res.end();
    break;
  default:
    res.writeHead(404);
    res.end();
  }
});

server.listen(8000);
```

<details><summary><b>HTML Portion:</b></summary>
<p>

```html
<!-- home.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Home</title>
  </head>
  <body>
    HOME
  </body>
</html>

<!-- about.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>About</title>
  </head>
  <body>
    ABOUT
  </body>
</html>
```

</p>
</details>

We can actually take a look at HTTP status codes using `http.STATUS_CODES`.

<details><summary><b>HTTP Status Code:</b></summary>
<p>

```bash
node
> http.STATUS_CODES
{ '100': 'Continue',
  '101': 'Switching Protocols',
  '102': 'Processing',
  '103': 'Early Hints',
  '200': 'OK',
  '201': 'Created',
  '202': 'Accepted',
  '203': 'Non-Authoritative Information',
  '204': 'No Content',
  '205': 'Reset Content',
  '206': 'Partial Content',
  '207': 'Multi-Status',
  '208': 'Already Reported',
  '226': 'IM Used',
  '300': 'Multiple Choices',
  '301': 'Moved Permanently',
  '302': 'Found',
  '303': 'See Other',
  '304': 'Not Modified',
  '305': 'Use Proxy',
  '307': 'Temporary Redirect',
  '308': 'Permanent Redirect',
  '400': 'Bad Request',
  '401': 'Unauthorized',
  '402': 'Payment Required',
  '403': 'Forbidden',
  '404': 'Not Found',
  '405': 'Method Not Allowed',
  '406': 'Not Acceptable',
  '407': 'Proxy Authentication Required',
  '408': 'Request Timeout',
  '409': 'Conflict',
  '410': 'Gone',
  '411': 'Length Required',
  '412': 'Precondition Failed',
  '413': 'Payload Too Large',
  '414': 'URI Too Long',
  '415': 'Unsupported Media Type',
  '416': 'Range Not Satisfiable',
  '417': 'Expectation Failed',
  '418': "I\'m a Teapot",
  '421': 'Misdirected Request',
  '422': 'Unprocessable Entity',
  '423': 'Locked',
  '424': 'Failed Dependency',
  '425': 'Unordered Collection',
  '426': 'Upgrade Required',
  '428': 'Precondition Required',
  '429': 'Too Many Requests',
  '431': 'Request Header Fields Too Large',
  '451': 'Unavailable For Legal Reasons',
  '500': 'Internal Server Error',
  '501': 'Not Implemented',
  '502': 'Bad Gateway',
  '503': 'Service Unavailable',
  '504': 'Gateway Timeout',
  '505': 'HTTP Version Not Supported',
  '506': 'Variant Also Negotiates',
  '507': 'Insufficient Storage',
  '508': 'Loop Detected',
  '509': 'Bandwidth Limit Exceeded',
  '510': 'Not Extended',
  '511': 'Network Authentication Required' }
```

</p>
</details>

## Parsing URLs and Query Strings
If you need to parse URL, Node has this URL module that you can use.

```bash
node
> url
{ Url: [Function: Url],
  parse: [Function: urlParse],
  resolve: [Function: urlResolve],
  resolveObject: [Function: urlResolveObject],
  format: [Function: urlFormat],
  URL: [Function: URL],
  URLSearchParams: [Function: URLSearchParams],
  domainToASCII: [Function: domainToASCII],
  domainToUnicode: [Function: domainToUnicode] }
```
It has many methods, but the most important one is `parse`.
The format method is also useful.

[NodeJS URL](https://nodejs.org/api/url.html)

![URL Diagram](https://user-images.githubusercontent.com/8571179/60070047-0e466280-9737-11e9-96f3-5f777e1d49e3.png)

This diagram here has details on all the elements of a URL. This example is manually parsing `http://user:pass@sub.example.com:8080/p/a/t/h?query=string#hash` URL in here.

For example, I can use the `url.parse` method to parse this URL.

```bash
url.parse('https://www.udemy.com/courses/search/?ref=home&src=ukw&q=java')
Url {
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'www.udemy.com',
  port: null,
  hostname: 'www.udemy.com',
  hash: null,
  search: '?ref=home&src=ukw&q=java',
  query: 'ref=home&src=ukw&q=java',
  pathname: '/courses/search/',
  path: '/courses/search/?ref=home&src=ukw&q=java',
  href:
   'https://www.udemy.com/courses/search/?ref=home&src=ukw&q=java'
}
```
this call is going to give me all the elements that I have in that URL.

We can actually also specify a second argument `true` to parse the query string itself.

<details><summary><b>specify a second argument true:</b></summary>
<p>

```bash
url.parse('https://www.udemy.com/courses/search/?ref=home&src=ukw&q=java', true)
Url {
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'www.udemy.com',
  port: null,
  hostname: 'www.udemy.com',
  hash: null,
  search: '?ref=home&src=ukw&q=java',
  query: { ref: 'home', src: 'ukw', q: 'java' }, // < --- here
  pathname: '/courses/search/',
  path: '/courses/search/?ref=home&src=ukw&q=java',
  href:
   'https://www.udemy.com/courses/search/?ref=home&src=ukw&q=java' }
```

</p>
</details>

Reading information from the query string is as easy as doing `.query.queue`, for example.

<details><summary><b>.query.queue:</b></summary>
<p>

```bash
url.parse('https://www.udemy.com/courses/search/?ref=home&src=ukw&q=java', true).query.q
'java'
```

</p>
</details>

If you have an object will all these elements detailed and you want to format this object into a URL, you can use the `url.format` method. And this will give you back a string with all these URL object properties concatenated in the right way.

<details><summary><b>using url.format():</b></summary>
<p>

```javasctipt
{
  protocol: 'https',
  host: 'www.udemy.com/courses',
  search: '?ref=home&src=ukw&q=java',
  pathname: '/search',
}
```

```bash
url.format({
...   protocol: 'https',
...   host: 'www.udemy.com/courses',
...   search: '?ref=home&src=ukw&q=java',
...   pathname: '/search',
... })
'https://www.udemy.com/courses/search?ref=home&src=ukw&q=java'
```

</p>
</details>

If you only care about the query string, then you can use the `querystring` module.
```bash
> querystring
{ unescapeBuffer: [Function: unescapeBuffer],
  unescape: [Function: qsUnescape],
  escape: [Function: qsEscape],
  stringify: [Function: stringify],
  encode: [Function: stringify],
  parse: [Function: parse],
  decode: [Function: parse] }
```
most important ones are the `parse method` and the `stringify method`.

querystring:
```javascript
{
  name: 'Khalid Syfullah',
  website: 'https://khalid547.wordpress.com/'
}
```

```bash
querystring.stringify({
...   name: 'Khalid Syfullah',
...   website: 'https://khalid547.wordpress.com/'
... })
'name=Khalid%20Syfullah&website=https%3A%2F%2Fkhalid547.wordpress.com%2F'
```

reverse method of querystring:

```bash
querystring.parse('name=Khalid%20Syfullah&website=https%3A%2F%2Fkhalid547.wordpress.com%2F')
{ name: 'Khalid Syfullah',
  website: 'https://khalid547.wordpress.com/' }
```