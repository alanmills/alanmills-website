# Removing mistakenly added files to the previous Git commit
**Author:** [Alan Mills]
**Date:** [17 January 2018 18:58]
**Tags:** [Git]
**Status**: Publish

When working with Git, sometimes you can start to make new changes before committing your previous change.  In the act of committing the changes, you run the command `git commit -am "my changes"`.

To fix this, you need to reset the Git history and re-commit the change.
1. `git reset --soft HEAD^` - reset the Git HEAD pointer to before the previous commit and preserve all changes as unstaged changes. 
2. `git reset HEAD path/to/unwanted_file` - remove the changed file from the list of staged changes
3. `git commit -m "my changes"` - commit the changes.  Note `-a` has been removed this time to ensure that we do not add back in `path/to/unwanted_file`.
