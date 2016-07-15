Using Visual Studio Team Services Build service to compile a project consuming a VSTS published NuGet package
=============================================================================================================
**Author:** Alan Mills  
**Date:** [1 June 2016 11:52](/blog/history/2016-06.md)  
**Tags:** [Visual Studio Team Services](/blog/categories/visual-studio-team-services.md), [NuGet](/blog/categories/nuget.md),
[Git](/blog/categories/git.md)  
**Status**: Draft

1. Add NuGet.config to your solution
2. [Update the Build to consume the NuGet.config file]

Add NuGet.config to your solution
---------------------------------
Add your feed as well as nuget.org - [Chained NuGet config files](http://docs.nuget.org/Consume/NuGet-Config-File) does not seem to be supported on VSTS.

Update the Build to consume the NuGet.config file
-------------------------------------------------
**NOTE**  You do not need to do anything with the security as there is a special build service identity that authenticates with the the VSTS Package manager, see [Authenticating to feeds with NuGet](https://www.visualstudio.com/docs/package/get-started/nuget/auth).

Update the Build step with the path to the NuGet.config file.
