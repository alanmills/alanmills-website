# AWS Lambda
**Author:** [Alan Mills]
**Date:** [16 June 2017 16:25]
**Tags:** [AWS], [AWS Lambda]
**Status**: Draft

``` javascript
exports.handler = (event, context, callback) => {
    callback(null, 'Hello, from Lambda);
};
```

event - information from what called you e.g. S3 or a HTTP request e.g. JSON body, etc.
context - system information
