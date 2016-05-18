Cleaning up master git commits with a remote feature branch
===========================================================
**Author**: Alan Mills  
**Date**: [10 May 2016 11:41](/blog/history/2016-05.md)
**Tags**: [Git](/blog/categories/git.md), [Visual Studio Team Services](/blog/categories/visual-studio-team-services.md)
**Status**: Draft

This post is a follow up to the post [Cleaning up master git commits to a local feature branch](blog/2016/05/cleaning-up-master-git-commits-to-a-local-feature-branch.md)

Sometimes you can find yourself making changes to a branch e.g. master that should really be in their own feature branch.  When working with [Visual Studio Team Services](/blog/categories/visual-studio-team-services.md), the ideal solution is to create feature/task/etc branches from Team Services web app.  Therefore you can clean up master by doing the following:
1. Creating a Feature branch in VSTS
2. Find out the head commit and last commit prior to making changes for the feature
3. Pulling the new remote feature branch locally
4. Move the head position for the remote branch
5. Move the head position for the original (master) branch
6. Checkout the feature branch and carryon working on the feature

[Git pulling a remote branch to local](blog/2015/11/git-pulling-remote-to-a-local-branch.md)

``` bash
$ git log
commit 88ab0be84f9e218fbfabfaca7ba6d3ffee058eba
Author: Alan Mills <alan.mills@cadamus.com>
Date:   Thu May 10 10:20:00 2016 +0100

    PR 125 merged in the previous feature

commit 396b3262b97e516f6b123eaae06fe1d813cee75f
Author: Alan Mills <alan.mills@cadamus.com>
Date:   Thu May 10 10:36:58 2016 +0100

    Setup initial project with DTOs.

$ git fetch
$ git checkout -b feature_branch_name remote-name/feature_branch_name
$ git reset --hard 396b3262b97e516f6b123eaae06fe1d813cee75f
$ git checkout master
$ git reset --hard 88ab0be84f9e218fbfabfaca7ba6d3ffee058eba
$ git checkout feature_branch_name
```
