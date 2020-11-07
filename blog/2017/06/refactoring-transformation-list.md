Refactoring Transformation List
==============================================
**Author:** [Alan Mills]
**Date:** [15 June 2017 10:51]
**Tags:** [Refactoring]
**Status**: Draft

1. Null
2. Null to Constant
3. Constant to Variable
4. Add Computation
5. Split Flow
6. Variable to array
7. Array to Container
8. If to While
9. Recurse
10. Iterate
11. Assign
12. Add Case


## Null
All functions start at null.  The steps in the Red, Gree, Refactor phase is to create the falling test
``` javascript
let assert = require('assert');

describe('Prime Factors:', function() {
  it('null returns []', assert.deepEqual(primeFactors(null), []));
});
```

Then we use the Null transformation to remove the **ReferenceError: primeFactors is not defined**

``` javascript 
let primeFactors = () = { return null; };
```

Of course the first test is still not *Green* however, we have completed the **Null** transform.

## Null to Constant
To get our first test to pass we need to use the **Null to Constant** transfrom to transform from the `null` being returned by `primeFactors` to an empty array `[]`.

``` javascript
let assert = require('assert');

describe('Prime Factors:', function() {
  it('null returns []', assert.deepEqual(primeFactors(null), []));
});


let primeFactors = () = { return []; };
```

This transform results in the first test going from **Red** to **Green**.


## Recursion
Use tail recursion

Functional JavaScript – Tail Call Optimization and Trampolines https://taylodl.wordpress.com/2013/06/07/functional-javascript-tail-call-optimization-and-trampolines/

Minimal Tail Calls https://jsperf.com/external-tail-calls/2
