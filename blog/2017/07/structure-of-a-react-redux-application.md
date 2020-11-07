# Structure of a React Redux application
**Author:** [Alan Mills]
**Date:** [14 July 2017 18:43]
**Tags:** [React], [Redux]
**Status**: Draft

``` bash
/ (root) - project configuration files for npm, babel, webpack, etc
  /build - the output location for the webpack build
  /app - the React app root
    /actions - 
    /components - React display components, these should contain no Redux code
    /constants - 
    /containers - Redux containers that align with the React components.  These manage the Redux state and pass what is required to the relevant Redux component
    /reducers
    index.jsx - the React app entry file
```
