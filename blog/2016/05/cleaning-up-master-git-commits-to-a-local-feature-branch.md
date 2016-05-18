Cleaning up master git commits to a local feature branch
========================================================
**Author**: Alan Mills  
**Date**: [10 May 2016 10:37](/blog/history/2016-05.md)
**Tags**: [Git](/blog/categories/git.md)
**Status**: Publish

Sometimes you can find yourself making changes to a branch e.g. master that should really be in their own local feature branch.  You can clean this up by doing the following:
1. [Find out the last commit prior to making changes for the feature](find-out-the-last-commit-prior-to-making-changes-for-the-feature)
2. [Create a new branch](create-a-new-branch)
3. [Move the head position for current branch (master) to the commit prior to star working on the feature](move-the-head-position-for-current-branch-(master)-to-the-commit-prior-to-star-working-on-the-feature)
4. [Checkout new branch and carry on working on the feature](checkout-new-branch-and-carry-on-working-on-the-feature)

If you are working with a central ALM tool like [Visual Studio Team Services](/blog/categories/visual-studio-team-services.md) then the steps are slightly different, as outlined in the post [Cleaning up master git commits with a remote feature branch](blog/2016/05/cleaning-up-master-git-commits-with-a-remote-feature-branch.md)


### Find out the last commit prior to making changes for the feature

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
```

From this output from ```git log```, the commit prior to working on the new feature is 88ab0be84f9e218fbfabfaca7ba6d3ffee058eba

### Create a new branch
``` bash
$ git branch feature/my-new-feature
```

As the HEAD position of the current (master) branch is pointing at commit 396b3262b97e516f6b123eaae06fe1d813cee75f, so is the new feature/my-new-feature branch.

### Move the head position for current branch (master) to the commit prior to star working on the feature
``` bash
$ git reset --hard 88ab0be84f9e218fbfabfaca7ba6d3ffee058eba
```

The HEAD position of the current (master) branch is now pointing at 88ab0be84f9e218fbfabfaca7ba6d3ffee058eba

### Checkout new branch and carry on working on the feature
``` bash
$ git checkout feature/my-new-feature
```

The HEAD position of branch feature/my-new-feature is still pointing at commit 396b3262b97e516f6b123eaae06fe1d813cee75f so you can now continue working with all previous changes intact.
