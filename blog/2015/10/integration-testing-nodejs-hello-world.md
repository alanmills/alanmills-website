Integration testing the standard Node.js Hello World sample
===========================================================
Author: Alan Mills
Date: 3 October 2015 18:29
Tags: integration testing, TDD, nodejs

Following the [Unit testing the standard Node.js Hello World sample](./unit-testing-nodejs-hello-world.md).  This post is focused on integration testing the Node.js Hello World sample

[The standard Node.js "hello world" example](https://nodejs.org/en/about/)
``` javascript
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, "127.0.0.1");

console.log('Server running at http://127.0.0.1:1337/');
```

## Running the Hello World Server

## Validating the header

## Validating the body
