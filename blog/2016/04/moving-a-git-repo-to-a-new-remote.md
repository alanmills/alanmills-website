Moving a Git repo to a new remote
=================================
**Author**: Alan Mills  
**Date**: [12 April 2016 18:29](/blog/history/2016-04.md)
**Tags**: [Git](/blog/categories/git.md)
**Status**: Draft

``` bash
$ git checkout master
$ git remote -v
$ git remote rm origin
$ git remote add origin https://New_Remote_Repository_Location
$ git remote -v
$ git push -u origin master
$ git pull origin master
```
