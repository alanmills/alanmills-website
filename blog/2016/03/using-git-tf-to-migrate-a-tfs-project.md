Using Git-TF to migrate a TFS project using Team Foundation Version Control
===========================================================================
**Author:** Alan Mills  
**Date:** [02 March 2016 15:49](/blog/history/2016-03.md)  
**Tags:** [Team Foundation Server 2013](/blog/categories/team-foundation-server-2013.md), [Git for Windows](/blog/categories/git-for-windows.md)


Steps

1. Install Git for Windows
2. Install Git-TF
3. Setup new TFS project
4. Clone existing TFS project
5. Configure local git project to work with new TFS project
6. Push project to new TFS project

## Install Git for Windows
Install [Git for Windows](https://git-for-windows.github.io), while writing this blog post this is [version 2.7.0](https://github.com/git-for-windows/git/releases/tag/v2.7.0.windows.1).

## Install Git-TF
``` PS
PS>
```

## Clone exiting TFS project
``` bash
$ cd /d/code/
$ git tf clone http://myserver:8080/tfs/collectionName $/TeamProject --deep  --mentions
```

### Clone the existing TFS project to a new local git repository
``` bash
$ git clone TeamProject NewTeamProject
```

### Configure the new git repository
``` bash
$ cd NewTeamProject
$ git tf configure http://newserver:8080/tfs/newCollectionName $NewTeamProject
```

### Merge new local repository with the new Team project
```bash
$ git tf pull --deep --ours
$ git commit -a -m "Merged TeamProject to NewTeamProject"
```

### Checkin project to new Team Project
``` bash
$ git tf checkin --deep --keep-author --mentions --autosquash
```

#### Configure USERMAP
There could be a miss-match between the author history in the TeamProject and NewTeamProject.







This will clone the TeamProject in to a folder called TeamProject within the D:\code folder
### Error Messages

git-tf: TF14019: The changeset ## does not exist

git-tf: TF14098: Access Denied User <<your name>> needs Checkin, CheckinOther permission(s) for $/path/to/file

### Update the Git-TF configuration
```bash
$ git tf configure --keep-author --deep --metadata
```

**do we need to touch all of the files? or add a new project to the collection?**

## Configure local git project to work with new TFS project
``` bash
$ cd TeamProject
$ git tf configure http://newserver:8080/tfs/newCollectionName $/NewTeamProject --deep  --force
$ git tf configure --list
```

## Pull new TFS project to local git repository
``` bash
$ git tf pull --deep --ours
```

## Push local git repository to new TFS project
```bash
$ git tf checkin  --deep --keep-author --mentions --autosquash
```
## USERMAP
Mapping existing repository to new repository

## References
[Git for Windows](https://git-for-windows.github.io)
[Visual Studio Team Services](https://www.visualstudio.com)
[Git-TF](https://gittf.codeplex.com)
