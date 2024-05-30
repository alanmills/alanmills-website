# Amend a pushed Git commit 

**Author:** [Alan Mills]
**Date:** [21 May 2024 21:16]
**Tags:** [Git]
**Status**: Draft

There are times that you forget to include all of the changes in a Git commit.  For instance you
complete the flow:

```bash
# edited file-that-i-forgot.txt
# edited file-that-i-remember.txt
git add file-that-i-remember.txt
git commit
git push
```

You then realise that you forgot to include some files for instance you run `git status` and you
get a surprise that there are untracked files or files with changes that you intended to include.

This can be be resolved by adding those files and amending your previous commit.

```bash
# realize you forgot a file
git add file-that-i-forgot.txt
git commit --amend --no-edit
```

However, you had `git push` your previous commit.  When you `--amend` your commit, you are not
changing the original commit, Git actually creates a new commit and replace the previous commit.

It could be that other people have made changes on top of your previoud commit, so you need to tell
your remote that you would like to replace the commit has that those other changes should reference.
This is done with `--force-with-lease`.

```bash
git push --force-with-lease
```

## Reference

* [Add files to last commit][amend]
* [push an amended commit][push]

[amend]: https://stackoverflow.com/a/37848143/1456748
[push]: https://stackoverflow.com/a/70051106/1456748