Mocking console.log with Sinon.JS
=================================
**Author:** Alan Mills  
**Date:** [4 October 2015 15:17](/blog/history/2015-10.md)  
**Tags:** [Test Driven Development](/blog/categories/test-driven-development.md), [Node.js](/blog/categories/node-js.md), [Chai Assertion Library](/blog/categories/chai-assertion-library.md), [Sinon.JS](/blog/categories/sinon-js.md)

# Stubbing console.log
``` javascript
var log = sinon.stub(console, 'log');

httpServer.simpleHttpServer();

log.restore();
assert.ok(log.calledWithExactly('Server running at http://127.0.0.1:1337/'));
```
