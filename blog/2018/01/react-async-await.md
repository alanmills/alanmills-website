# React Async Await Example
**Author:** [Alan Mills]
**Date:** [8 January 2018 22:28]
**Tags:** [JavaScript], [React]
**Status**: Draft

``` JavaScript
import React from 'react';
import ReactDOM from 'react-dom';
import 'babel-polyfill';

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { answer: 42 };
  }

  asyncFunc() {
    return new Promise(resolve => {
      setTimeout(() => { resolve(37); }, 2000);
    });
  };

  async componentDidMount() {
    this.setState({ answer: await this.asyncFunc() });
  }

  render() {
    return (
      <h2>Hello Express, Ejs and React -- { this.state.answer }</h2>
    );
  }
}

ReactDOM.render(<App />, document.getElementById('app'));
```

## Error messages
### Using the function keyword in a Class definition
``` bash
Hash: f1fe6a3367a14dfbba0e
Version: webpack 3.10.0
Time: 72ms
    Asset    Size  Chunks                    Chunk Names
bundle.js  932 kB       0  [emitted]  [big]  main
   329 modules

ERROR in ./lib/components/Index.js
Module build failed: SyntaxError: Unexpected token (11:11)

   9 |   }
  10 | 
> 11 |   function asyncFunc() {
     |            ^
  12 |     return new Promise(resolve => {
  13 |       setTimeout(() => { resolve(37); }, 2000);
  14 |     });

 @ multi babel-polyfill ./lib/components/Index.js
 ```

 ### Using await without marking the function async
 ``` bash
 Version: webpack 3.10.0
Time: 82ms
    Asset    Size  Chunks                    Chunk Names
bundle.js  932 kB       0  [emitted]  [big]  main
   329 modules

ERROR in ./lib/components/Index.js
Module build failed: SyntaxError: await is a reserved word (18:28)

  16 | 
  17 |   componentDidMount() {
> 18 |     this.setState({ answer: await this.asyncFunc() });
     |                             ^
  19 |   }
  20 | 
  21 |   render() {

 @ multi babel-polyfill ./lib/components/Index.js
 ```