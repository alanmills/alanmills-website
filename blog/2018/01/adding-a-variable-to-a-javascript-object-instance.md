# Adding a variable to a JavaScript object instance
**Author:** [Alan Mills]
**Date:** [26 January 2018 13:31]
**Tags:** [JavaScript]
**Status**: Draft



``` javascript
Object.assign();

const obj = {};
obj.v1 = 'variable 1';  // { v1: 'variable 1' }
obj['v2'] = 'variable 2'; // { v1: 'variable 1' } - what happened to v2?
Object.getOwnPropertyNames(obj); // [ 'v1', 'v2']
````


