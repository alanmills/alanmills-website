Delete a Git tag locally and remotely
=====================================
**Author**: Alan Mills  
**Date**: [19 April 2016 16:00](/blog/history/2016-04.md)   
**Tags**: [Git](/blog/categories/git.md)  
**Status**: Draft

``` bash
$ git fetch
$ git tag
1234-my-features-branch-that-does-something
v1.0
v1.1

$ git tag -d '1234-my-features-branch-that-does-something'
$ git push origin ':/refs/tags/1234-my-features-branch-that-does-something'
```
