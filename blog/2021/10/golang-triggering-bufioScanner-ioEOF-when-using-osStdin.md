# Git merge the last two commits into a single commit
Golang triggering bufio.Scanner io.EOF when using os.Stdin

**Author:** [Alan Mills]
**Date:** [26 September 2021 18:13]
**Tags:** [Git]
**Status**: Draft

## Overview

From time to time you will want to commit a change to your local working branch and realise that you have forgotten to include all of the changes.  One approach to resolve this is to commit the rest of the changes and merge the two commits into a single commit.  **NOTE: these steps assume that you have not pushed the changes to the remote, as this will mess up the commit history for everyone else using the Git Repo**.

Start an interactive git rebase action telling Git that you want the last two commits `git rebase -i HEAD~2`.

Git will use the default editor (normally vim on Mac and Linux) to display the last two commit messages, in the order oldest..newest.

```vim
pick 547f3f6 some of the changes that I want to share with the team
pick 5128f64 My change that I want to share with the team
```

Making the assumption that you want to keep commit message from `HEAD`, complete the steps:

1. Swap the order of the two lines
2. Update `HEAD~1` from `pick` to `fixup`.

The history should now look like

```vim
pick 5128f64 My change that I want to share with the team
fixup 547f3f6 some of the changes that I want to share with the team
```

Now, save and quit the interactive rebase file (assuming vim `:wq`)

This change has the affect of squashing this change leaving only the `pick` log message in your log history.

## References

* [How to Combine Multiple Git Commits into One](https://www.w3docs.com/snippets/git/how-to-combine-multiple-commits-into-one-with-3-steps.html)
