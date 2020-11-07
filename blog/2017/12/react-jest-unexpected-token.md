# React Jest unexpected token error
**Author:** [Alan Mills]
**Date:** [21 December 2017 18:49]
**Tags:** [React], [Jest]
**Status**: Draft

While getting started learning Jest Snapshot Testing following the [Snapshot Testing guide](http://facebook.github.io/jest/docs/en/snapshot-testing.html).  Each time I ran `npm run test`, Jest was ouputting the error **Unexpected token**.  The console was outputing the source of the error as the React JSX JavaScript syntax extension.  While the error message does not provide any explination of why this is an error, the reason was was because you have to install and configure Babel for Jest to call.

To fix:
1. Install the npm dev dependencies `npm install --save-dev babel-jest babel-preset-env babel-preset-react`
2. Create the configuration file `.babelrc`
``` json
{
  "preset": ["env", "react"]
}
``` 

## Jest console error message
``` console
FAIL  __tests__/link.react.test.js
  ● Test suite failed to run

    /Users/cadamus/code/Learning/React/Jest/snapshot/__tests__/link.react.test.js: Unexpected token (6:31)
        4 | 
        5 | it('renders correctly', () => {
      > 6 |   const tree = renderer.create(<Link />).toJSON();
          |                                ^
        7 |   expect(tree).toMatchSnapshot();
        8 | });
        9 | 

Test Suites: 1 failed, 1 total
Tests:       0 total
Snapshots:   0 total
Time:        0.713s
Ran all test suites.
npm ERR! Test failed.  See above for more details.
```
