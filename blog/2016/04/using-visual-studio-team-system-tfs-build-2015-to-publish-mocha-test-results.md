Using Visual Studio Team System, TFS Build 2015 to publish Mocha test results
=============================================================================
**Author:** Alan Mills  
**Date:** [19 April 2016 12:58](/blog/history/2016-04.md)  
**Tags:** [Visual Studio Team Services](/blog/categories/visual-studio-team-services.md), [Mocha](/blog/categories/mocha.md),
[Grunt](/blog/categories/grunt.md)  
**Status**: Draft

Grunt file
----------

``` json
files: {
  test: { src: '' },
  testResultsFile: { src: '' }
},
mochaTest: {
  development: {
    options: {
      reporter: 'spec'
    },
    src: '<%= files.test.src %>'
  },
  unit: {
    options: {
      reporter: 'XUnit',
      captureFile: <%= files.testResultsFile.src %>,
      quiet: true
    },
    src: '<%= files.test.src %>'
  }
},
```

VSTS Build - 'Publish Test Results'
-----------------------------------

| Setting            | Value         |
|--------------------|---------------|
| Test Result Format | JUnit         |
| Test Results files | **/TEST-*.xml |
| Merge Test Results | False         |
| Test Run Title     | Unit tests    |
