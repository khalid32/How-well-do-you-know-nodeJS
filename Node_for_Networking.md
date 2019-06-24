# Node for Networking:

## TCP Networking with the Net Module
```javascript
const server = require('net').createServer();

server.on('connection', socket => {
  console.log('Client connected');
  socket.write('Welcome new client!\n');

  socket.on('data', data => {
    console.log('data is:', data);
    socket.write('data is: ');
    socket.write(data);
  });

  socket.on('end', () => {
    console.log('Client disconnected');
  });
});

server.listen(8000, () => console.log('Server bound'));
```

The event handler also gives us access to a connected socket itself. This socket object implements a `duplex stream interface`, which means that we can read and write to it.

The socket being a duplex stream means that it's also an EventEmitter. So we can listen to data event on the socket. The handler for this event gives us access to a buffer object.

## Working with Multiple Sockets
```javascript
const server = require('net').createServer();
let counter = 0;
let sockets = {};

server.on('connection', socket => {
  socket.id = counter++;
  sockets[socket.id] = socket;

  console.log('Client connected');
  socket.write('Welcome new client!\n');

  socket.on('data', data => {
    Object.entries(sockets).forEach(([, cs]) => {
      cs.write(`${socket.id}: `);
      cs.write(data);
    });
  });

  socket.on('end', () => {
    delete sockets[socket.id];
    console.log('Client disconnected');
  });
});

server.listen(8000, () => console.log('Server bound'));
```

## The DNS Module

We can use `DNS Module` to translate network names to addresses and vice versa. For example, we can use the lookup method to lookup a host.
```javascript
const dns = require('dns'); // name -- addresses

// == 1st Example ==
dns.lookup('pluralsight.com', (err, address) => {
  console.log(address);
});

// == 2nd Example ==
dns.resolve4('pluralsight.com', (err, address) => {
  console.log(address);
});

// == 3rd Example ==
dns.resolveMx('pluralsight.com', (err, address) => {
  console.log(address);
});

// == 4th Example ==
dns.reverse('35.161.75.227', (err, hostnames) => {
  console.log(hostnames);
});
```

The lookup method on the DNS module is actually a very special one, because it does not necessarily perform any network communication, and instead uses the underlying operating system facilities to perform the resolution.
This means that it will be using `libuv threads`. All the other methods on the DNS module uses the network directly and does not use libuv threads.

In 2nd example, the equivalent network method for lookup is `resolve4`, 4 is the family of IP address, so we're interested in IPv4 addresses. This will give us an array of addresses in case the domain has multiple A records.

## UDP Datagram Sockets
```javascript
const dgram = require('dgram');
const PORT = 3333;
const HOST = '127.0.0.1';

// Server
const server = dgram.createSocket('udp4');

server.on('listening', () => console.log('UDP Server listening'));

server.on('message', (msg, rinfo) => {
  console.log(`${rinfo.address}:${rinfo.port} - ${msg}`);
});

server.bind(PORT, HOST);

// Client

const client = dgram.createSocket('udp4');
const msg = Buffer.from('Pluralsight rocks');

client.send(msg, 0, msg.length, PORT, HOST, (err) => {
  if (err) throw err;

  console.log('UDP message sent');
  client.close();
});
```