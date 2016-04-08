Use PsGet to setup PoshGit - a PowerShell CLI for Git
=====================================================
**Author:** Alan Mills  
**Date:** [29 January 2016 16:47](/blog/history/2016-01.md)  
**Tags:** [PowerShell](/blog/categories/powershell.md), [Windows 10](/blog/categories/windows-10.md), [Git for Windows](/blog/categories/git-for-windows.md)  
**Status**: Draft

Git is not a natural bedfellow on Windows however, with some setup and configuration working with Git on Windows is almost as good as on the Mac/Linux.

1. Install Git for Windows
1. Run ISE in Administrator mode
2. Change the Execution Policy to RemoteSigned
3. Download and execute [PsGet](http://psget.net/)
4. Install PoshGit
5. Start using Git

### Install Git for Windows
Install [Git for Windows](https://git-for-windows.github.io), while writing this blog post this is [version 2.7.0](https://github.com/git-for-windows/git/releases/tag/v2.7.0.windows.1).

### Run ISE in Administrator mode


### Change the Execution Policy to RemoteSigned
``` PowerShell
PS>  Set-ExecutionPolicy RemoteSigned
```
### Download and execute PsGet
``` PowerShell
(New-Object Net.WebClient).DownloadString("http://psget.net/GetPsGet.ps1") | iex
```

### Install PoshGit
[posh-git](https://github.com/dahlbyk/posh-git)
``` PowerShell
Install-Module posh-git
```

### Start using Git
``` PowerShell

```
