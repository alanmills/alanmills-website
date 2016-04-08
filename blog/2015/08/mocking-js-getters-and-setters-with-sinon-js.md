Mocking JavaScript Getter and Setters with Sinon.JS
===================================================
**Author:** Alan Mills  
**Date:** [27 August 2015 11:56](/blog/history/2015-08.md)  
**Tags:** [Testing](/blog/categories/testing.md), [Sinon.JS](/blog/categories/sinon.md), [JavaScript](/blog/categories/javascript.md)   
**Status**: Draft

Need to work out how to great an extension for Sinon to wrap around JavaScript getters and setters so that you can assert that they have been set/got, etc.

``` JavaScript
should = require('chai').should(),
    sinon = require('sinon');

  describe('enhancedHttpServer tests.', function() {
    var listen,
      createServer;

    beforeEach(function() {
      listen = sinon.stub();
      createServer = sinon.stub(require('http'), 'createServer').returns({listen: listen});
    });

      it('should access the request url property', function() {
        var getUrl = sinon.stub().returns('/test.txt'),
          request = { get url() { return getUrl(); } };

        var onRequest = createServer.callsArgWith(0, request);
        httpServer.enhancedHttpServer();

        getUrl.calledOnce.should.be.ok;
      });
```
