# Webpack ERROR in JSON file, Module build failed: SyntaxError: Unexpected token, expected ; 
**Author:** [Alan Mills]
**Date:** [8 January 2018 18:25]
**Tags:** [Webpack], [JSON], [JavaScript]
**Status**: Publish

I recently was developing a simple React app and decided to use the ES6 `import { data } from 'testData.json'` to load the JSON data into my application.  Webpack was outputting `ERROR in ./lib/testData.json Module build failed: SyntaxError: Unexpected token, expected ; (2:8)`.  

Initially, I could not think what was causing this, turned out that the mistake was in my `webpack.config.js` file.  The JavaScript `babel-loader` module rule was missing the `$` in the `test` regular expressions, and therefore, correctly Webpack was passing `babel-loader` my `testData.json` file.


## Webpack shell ERROR message
``` bash
$ webpack -wd

Webpack is watching the files…

Hash: cd6b19ff6b5a3693a74d
Version: webpack 3.10.0
Time: 3321ms
    Asset     Size  Chunks                    Chunk Names
bundle.js  2.82 MB       0  [emitted]  [big]  main
  [98] (webpack)/buildin/global.js 509 bytes {0} [built]
 [140] multi babel-polyfill ./lib/components/Index.js 40 bytes {0} [built]
 [356] ./lib/DataApi.js 1.44 kB {0} [built]
 [357] ./lib/testData.json 618 bytes {0} [built] [failed] [1 error]
    + 354 hidden modules

ERROR in ./lib/testData.json
Module build failed: SyntaxError: Unexpected token, expected ; (2:8)

  1 | {
> 2 |   "data": {
    |         ^
  3 |     "articles": [
  4 |       {
  5 |         "id": "95c12a8f6c88953ca8f8a39da25546e6",

 @ ./lib/components/App.js 17:16-38
 @ ./lib/components/Index.js
 @ multi babel-polyfill ./lib/components/Index.js
```

## Valid Webpack config file
``` JavaScript
const path = require('path');

const distPath = path.resolve(__dirname, 'public');
const appEntryFile = path.resolve(__dirname, 'lib/components/Index.js');
const appPath = ['babel-polyfill', appEntryFile];

module.exports = {
  entry: appPath,
  output: {
    filename: 'bundle.js', 
    path: distPath
  },
  devtool: 'source-map',
  module: {
    rules: [
      { test: /\.jsx?$/, exclude: /node_modules/, loader: ['babel-loader'] }
    ]
  } 
};  
```
