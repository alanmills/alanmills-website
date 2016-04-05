Refactoring Mocha tests to dynamically test Node.js Hello World sample
======================================================================
Author: Alan Mills
Date: 5 October 2015 19:17
Tags: unit testing, TDD, nodejs, mocha, chai, sinon

Following the [Unit testing the standard Node.js Hello World sample](./unit-testing-nodejs-hello-world.md).  This post covers refactoring the unit tests written for the Node.js Hello World sample to allows for the simultaneously testing of the simple hello world sample and an enhanced http server.

## Enhanced HTTP Server
The enhance HTTP Server allows the user to display the contents of a file. **Note due to security concerns, this example should never be used for a production application**
**TODO**
``` javascript
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, "127.0.0.1");

console.log('Server running at http://127.0.0.1:1337/');
```

## Dynamic Mocha tests

## Refactoring the Node.js Hello World sample unit tests
httpServerTest.js


## Applying the same test to enhanced HTTP Server
``` javascript
var httpServer = require('../../js/httpServer/enhancedHttpServer.js'),
  httpServerTest = require('./httpServerTest');

httpServerTest.createHttpServer(httpServer.enhancedHttpServer);
```
