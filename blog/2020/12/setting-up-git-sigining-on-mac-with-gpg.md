# Setting up Git sigining on Mac with GPG

**Author:** [Alan Mills]
**Date:** [20 December 2020 10:29]
**Tags:** [Git, GPG]
**Status**: Draft


[Setting up multiple git accounts](./setting-up-multiple-git-accounts.md)

## Export GPG Key

```bash
gpg --armor --export <GPG Key ID> | pbcopy
```  

## SSH Config

~/.ssh/config


## References

* [Generating a new GPG key](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-gpg-key)
