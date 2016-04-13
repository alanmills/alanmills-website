Delete a Git branch locally and remotely
========================================
**Author**: Alan Mills  
**Date**: [12 April 2016 21:59](/blog/history/2016-04.md)   
**Tags**: [Git](/blog/categories/git.md)  
**Status**: Draft

``` bash
$ git branch -a

* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/unwantedBranch

$ git push origin --delete unwantedBranch
```
