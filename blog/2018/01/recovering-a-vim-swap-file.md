# Recovering a Vim swap file 
**Author:** [Alan Mills]
**Date:** [15 January 2018 18:38]
**Tags:** [Vim]
**Status**: Draft

When opening a file, sometimes you will encounter the **E325: ATTENTION** warning message.  The warning occurs when Vim detects a swap file when opening a file, the swap file is a result of an unclosed buffer.  To determine if you should keep the original file or the changes in the swap file, I compare the contents of both. 

## Recovering a Vim swap file
The steps are:
1. Recover the swap file by pressing `r` at the **E325: ATTENTION** warning message
2. Save the swap file to a temporary location `:sav! /tmp/%`
3. Vertically split Vim `:vs`
4. Tell Vim to include the swap file in a diff process `:diffthis`
5. Move focus to the right vertical split, <kbd>Ctrl</kbd>+<kbd>w</kbd>, <kbd>Ctrl</kbd>+<kbd>l</kbd>
6. Edit the saved file, by instructing Vim to move to the previous buffer `:bp`, vim will redisplay the **E325: ATTENTION** warning message, where you press 'e'
7. Tell Vim to include the original file in a diff process `:diffthis`

Once completing these steps Vim will be in diff mode with the screen, vertical split with the swap file on the left and the original file on the right.

### Files are the same
When the files are the same
### Save the buffer version
### Save the original version
:bp
d


## Test this out yourself
The steps to create a swap file to recover are:
1. Create a swap file
2. Kill the vim process
3. Recover the file

### Create the swap file
1. From the terminal, create a new file `vim test.md`
2. Change into insert mode `i`
3. Enter some text `Hello, vim swap`
4. Exit edit mode <kbd>esc</kbd>
5. Save the file `:w`
6. Go back into insert mode `i`
7. Add some more text `file`
8. Stop the `vim test.md` job, <kbd>Ctrl</kbd>+ <kbd>z</kbd>

### Kill the vim process
1. In the terminal, kill the `vim test.md` job, ``kill -9 `jobs -p` ``

### Recover the file
1. Follow the steps at the top of this blog post.
