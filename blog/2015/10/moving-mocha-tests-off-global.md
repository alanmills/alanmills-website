Moving mocha tests off global variable space
============================================
**Author:** Alan Mills  
**Date:** [7 October 2015 23:30](/blog/history/2015-10.md)  
**Tags:** [Test Driven Development](/blog/categories/test-driven-development.md), [Node.js](/blog/categories/node-js.md), [Chai Assertion Library](/blog/categories/chai-assertion-library.md), [Sinon.JS](/blog/categories/sinon-js.md), [Mocha](/blog/categories/mocha.md)  
**Status**: Draft

[Mocah](http://mochajs.org) is a great JavaScript test framework however, all of the test examples place all of the tests into global variables.

**first test mocking a framework using sinon***

**second test mocking the same framework with sinon**

***ERROR: framework already wrapped... Mocha documentation states that they are independent...*** **What to do?**

**use functional blocks to wrap the 'independent' tests**
***Set the function to auto-execute***
