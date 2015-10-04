mocking-consolelog-with-sinonjs

Mocking console.log with Sinon.JS
=================================
Author: Alan Mills
Date: 4 October 2015 15:17
Tags: unit testing, TDD, nodejs, chai, sinon

# Stubbing console.log
``` javascript
var log = sinon.stub(console, 'log');

httpServer.simpleHttpServer();

log.restore();
assert.ok(log.calledWithExactly('Server running at http://127.0.0.1:1337/'));
```
