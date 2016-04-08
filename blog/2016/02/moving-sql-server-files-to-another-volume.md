Moving SQL Server files to another volume
=========================================
**Author:** Alan Mills  
**Date:** [24 February 2016 15:25](/blog/history/2016-02.md)  
**Tags:** [SQL Server](/blog/categories/sql-server.md)  
**Status**: Draft

If you install SQL Server using the defaults then all of your databases will be installed with SQL Server on the primary volume.  This is okay if you are lightly using SQL Server, performance is not a consideration and your primary volume is large.  If any of these is not the case then this is a bad plan and eventually you are going to need to sort things out.

In this article I am not going to cover SQL Server best practice for distributing

To get all of the available SQL PowerShell CommandLets
``` PS
PS> Get-Command - Module 'SQLPS'
```
[Running SQL Server PowerShell](https://technet.microsoft.com/en-us/library/cc281962(v=sql.105).aspx)

``` PS
$db = Get-SqlDatabase -ServerInstance 'ServerName\DBServer' -Name 'MyDatabase'
$fg = $db.FileGroups
$f = $fg.Files
$db.SetOffline()
Move-Item -Path $f.FileName -Destination 'D:\SQLData\DBSServer\DATA'
```

www.mikefal.net/tag/sqlps

[Move User Databases](msdn.microsoft.com/en-us/library/ms345483.aspx)
[Database Engine PowerShell Reference](msdn.microsoft.com/en-us/library/hh230864.aspx)
[SQL Server PowerShell](msdn.microsoft.com/en-us/library/hh245198.aspx)
[SQL Server Management Objects Reference](msdn.microsoft.com/en-us/library/mt571730.aspx)
