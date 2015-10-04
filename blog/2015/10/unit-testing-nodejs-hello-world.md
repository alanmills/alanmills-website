Unit testing the standard Node.js Hello World sample
====================================================
Author: Alan Mills
Date: 1 October 2015 12:07
Tags: unit testing, TDD, nodejs, chai, sinon

[The standard Node.js "hello world" example](https://nodejs.org/en/about/)
``` javascript
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, "127.0.0.1");

console.log('Server running at http://127.0.0.1:1337/');
```

## Stubbing http.createServer

## Stubbing http.ServerResponse

# Stubbing console.log
``` javascript
var log = sinon.stub(console, 'log');

httpServer.simpleHttpServer();

log.restore();
assert.ok(log.calledWithExactly('Server running at http://127.0.0.1:1337/'));
```

Following on from this post, is the [Integration testing the standard Node.js Hello World sample](./integration-testing-nodejs-hello-world.md)
