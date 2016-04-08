Open multiple copies of Microsoft Test Manager 2013 at the same time
=====================================================================
**Author:** Alan Mills  
**Date:** [14 July 2015 16:41](/blog/history/2015-07.md)  
**Tags:** [Windows 7](/blog/categories/windows-7.md), [Visual Studio 2013](/blog/categories/visual-studio-2013.md), [Microsoft Test Manager 2013](/blog/categories/microsoft-test-manager-2013.md)


[Microsoft Test Manager](https://msdn.microsoft.com/en-us/library/jj645157.aspx) is a great tool for planning and execute test on Windows however, by default you can only have a single instance running which can make certain tasks more difficult.

You can get round this by following the following steps:
1. Find the installed location of MTM e.g.``` C:\Program Files (x86)Microsoft Visual Studio 12.0\Common7\IDE\```.
2. Copy the files ``` mtm.exe ``` and ```mtm.exe.config``` in to the same folder as ``` mtm.exe ``` and name them ``` mtm2.exe ``` and ```mtm2.exe.config``` respectively.
3. Now run ``` mtm.exe ``` and ``` mtm2.exe ```.

You will now have two copies of MTM running allowing you to share and compare the same and different parts of Microsoft Test Manager 2013.
