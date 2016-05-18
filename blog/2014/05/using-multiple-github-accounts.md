Using multiple gitHub accounts without SSH
==========================================
**Author**: Alan Mills  
**Date**: [06 May 2014](/blog/history/2014-05.md)   
**Tags**: [Git](/blog/categories/git.md), [GitHub](/blog/categories/github.md), [OS X Mavericks](/blog/categories/osx-10-10.md), [Visual Studio Team Services](/blog/categories/visual-studio-team-services.md)
**Status**: Draft

The information that led me to solve my issue with using multiple accounts with [Visual Studio Team Services](/blog/categories/visual-studio-team-services.md), is the Wiki post by Freek Dijkstram, [Git Passwords in the Keychain](http://www.macfreek.nl/memory/Git_Passwords_in_the_Keychain)


Setup OSX to use multiple accounts from the same Website
--------------------------------------------------------
### Validate that the OSX KeyChain Credentials helper is installed
``` bash
$ git help -a | grep credential-
  credential-osxkeychain  subtree
```

### Setup Git to use multiple Accounts at the Same Website
``` bash
$ git config --global credential.useHttpPath true
$ git config --global -l
user.name=Alan Mills
user.email=alan@alanmills.info
credential.usehttppath=true
```

Configure each repository
-------------------------
 
