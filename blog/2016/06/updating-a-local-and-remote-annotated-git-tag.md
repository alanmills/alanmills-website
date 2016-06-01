Updating a local and remote annotated Git tag
=============================================
**Author**: Alan Mills  
**Date**: [1 June 2016 11:41](/blog/history/2016-06.md)
**Tags**: [Git](/blog/categories/git.md)
**Status**: Draft

To move a Git Tag from a previous commit to the current one, run the command `git tag -fa tag_name`

To then push this updated annotated tag to the remote server, run the command `git push origin --tags -f`
