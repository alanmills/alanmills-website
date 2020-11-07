# Python development using VIM - PEP 8 - Setting up spaces and indentention
*Author:** [Alan Mills]
**Date:** [12 October 2020 20:21]
**Tags:** [Vim, Python]
**Status**: Draft

When working developing Python code, we want to follow the [PEP 8 -- Style guidelines for Python Code](https://www.python.org/dev/peps/pep-0008/).

~/.vimrc
filetype plugin indent on

~/.vim/after/ftplugin/python.vim
set expandtab
set shiftwidth=4
set softtabstop=4
set autoindent
