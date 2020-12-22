# Setting up multiple git accounts

**Author:** [Alan Mills]
**Date:** [18 December 2020 10:15]
**Tags:** [Git]
**Status**: Draft


## Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "alan@company1.com" -f ~/.ssh/id_github_company1_ed25519
```

## SSH Config

```bash
~/.ssh/config
Host github_company1
  Host github.com
  IdentityFile ~/.ssh/id_github_comany1_ed25519
  User alan@company1.com
  IdentityOnly yes
  UseKeychain yes
```

## Upload SSH Key to GitHub

```bash
pbcopy < ~/.ssh/id_github_company1_ed25519.pub
```

[GitHub Account Settings Keys](https://github.com/settings/keys)


## Inspect SSH Key Fingerprint

```bash
ssh-keygen -l -f ~/.ssh/id_github_company1
```

## Test SSH Key

```bash
ssh -T git@github_company1
Enter passphrase for key '~/.ssh/id_github_company1_ed25519'
Hi alan-company1! You've successfully authenticated, but GitHub does not proved shell access.
```

## Clone when passing local config to git clone

```bash
git clone --config user.name="Alan Mills" --config user.email="alan@company1.com" git@github_company1:company1/repo.git
```

## Using Git profiles

```bash
~/.gitconfig
[includeIf "gitdir:~/code/github/company1/"]
  path=~/.git/github-company1.conf
```

```bash
gpg --list-secret-keys --keyid-format LONG alan@company1.com
sec   rsa4096/3AA5C34371567BD2 2020-12-18 [expires: 2024-12-18]
uid                          Alan Mills <alan@company1.com>
ssb   rsa4096/42B317FD4BA89E7A 2020-12-18 [E] [expires: 2024-12-18]
```

```bash
~/.git/github-company1.conf
  [user]
    name = Alan Mills
    email = alan@company1.com
    signingkey = 3AA5C34371567BD2 
  [commit]
    gpgsign = true
```

## References

* [Using multiple SSH keys for different Git providers by Max Yermakhanov](https://medium.com/objectsharp/using-multiple-ssh-keys-for-different-git-providers-f12045dae354)
* [Managing multiple Git profiles](https://deepsource.io/blog/managing-different-git-profiles/)
* [SSH Config File](https://www.ssh.com/ssh/config/)
* [How To: Inspect SSH Key Fingerprints](https://www.unixtutorial.org/how-to-inspect-ssh-key-fingerprints/)
* [Managing multiple Git profiles](https://deepsource.io/blog/managing-different-git-profiles/)
