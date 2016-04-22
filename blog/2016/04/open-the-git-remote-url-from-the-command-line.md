Open the git remote url from the command line
=============================================
**Author**: Alan Mills  
**Date**: [19 April 2016 16:38](/blog/history/2016-04.md)
**Tags**: [Bash](/blog/categories/bash.md), [Git](/blog/categories/git.md), [OS X El Capitan](/blog/categories/osx-10-11.md), [Windows 10](/blog/categories/windows-10.md)
**Status**: Draft

I often want to open the website associated with the Git repository that I am working with and getting the URL is covered in the blog post [What is the Git remote URL?](/blog/2016/04/what-is-the-git-remote-url.md).


To save you copying and pasting the URL from the terminal in to your browser, you can run the following commands from the terminal on Mac or Windows.

Mac
---
``` bash
$ open `git remote get-url origin`
```

Windows
-------
``` bash
$ start `git remote get-url origin`
```

**This works on Windows with VSTS.  Does not work on Mac (git version?) and with GitHub**
