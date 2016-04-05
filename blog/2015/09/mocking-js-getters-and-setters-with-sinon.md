Mocking JavaScript Getter and Setters with Sinon
================================================
Author: Alan Mills
Date: 27 August 2015 11:56
Tags: Testing, Sinon, JavaScript, Getter, Setters

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
