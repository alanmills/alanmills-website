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

## Clone using a Git profiles

```bash
~/.gitconfig
[includeIf "gitdir:~/code/github/company1/"]
  path=~/.git/github-company1.conf
```

**includeIf** if the end of the *gitdir* path ends with a **/**, then git will automatically apply the /** path wildcard. 


## Generate GPG Key

```bash
gpg --full-generate-key
Type of key: (1) RSA and RSA (default)
Keysize number of bits: 4096
Now long the key should be valid: 0 = key does not expire
Real name: Alan Mills
Email: alan@company1.com
Comment: GitHub.com
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: key 584360934W2H2G30 marked as ultimately trusted
gpg: revocation certificate stored as '/Users/alan/.gnupg/openpgp-revocs.d/4C634D942K2AB392405EN049584360934W2H2G30.rev'
public and secret key created and signed.

pub   rsa4096 2020-12-18 [SC]
      4C634D942K2AB392405EN049584360934W2H2G30
uid                      Alan Mills (GitHub.com) <alan@company1.com>
sub   rsa4096 2020-12-18 [E]

```

## Upload GPG Public Key to GitHub

In the previous step, when gpg generated the GPG key, you can get the information by using the `gpg --list-secret-keys` command. 

```bash
gpg --list-secret-keys --keyid-format LONG alan@company1.com
sec   rsa4096/584360934W2H2G30 2020-12-18 [SC]
      4C634D942K2AB392405EN049584360934W2H2G30
uid                 [ultimate] Alan Mills (GitHub.com) <alan@company1.com>
ssb   rsa4096/02XSB01LW30310L3 2020-12-18 [E]

gpg --armor --export 584360934W2H2G30 | pbcopy 
```

1. Open [GitHub Account Settings Keys](https://github.com/settings/keys)
2. Follow the instructions: [GitHub Docs - Adding a new GPG key to your GitHub account](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account)
   1. Click the **New GPG key** button
   2. Copy the GPG Public key to the system clipboard `gpg --armor --export 584360934W2H2G30 | pbcopy`
   2. Paste the public key into the **Key** text field
   3. Press the **Add GPG key** button

## Sign commit messages using PGP and force the use of SSH

```bash
~/.git/github-company1.conf
  [user]
    name = Alan Mills
    email = alan@company1.com
    signingkey = 584360934W2H2G30 
  [commit]
    gpgsign = true
  [url "ssh://git@github_company1"]
    insteadOf = ssh://git@github.com
  [url "ssh://git@github_company1"]
    insteadOf = https://github.com
```

## References

* [Using multiple SSH keys for different Git providers by Max Yermakhanov](https://medium.com/objectsharp/using-multiple-ssh-keys-for-different-git-providers-f12045dae354)
* [Managing multiple Git profiles](https://deepsource.io/blog/managing-different-git-profiles/)
* [SSH Config File](https://www.ssh.com/ssh/config/)
* [How To: Inspect SSH Key Fingerprints](https://www.unixtutorial.org/how-to-inspect-ssh-key-fingerprints/)
* [Managing multiple Git profiles](https://deepsource.io/blog/managing-different-git-profiles/)
* [git Configuration File](https://git-scm.com/docs/git-config#_configuration_file)
