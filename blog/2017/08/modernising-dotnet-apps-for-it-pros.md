# Modernising .NET Apps for IT Pros
**Author:** [Alan Mills]
**Date:** [16 August 2017 17:36]
**Tags:** [Windows Server 2016], [Docker], [.NET]
**Status**: Draft

[Docker youtube video](https://www.youtube.com/watch?v=gaJ9PzihAYw)

Using Docker to move Windows .NET apps into Docker without needing to change the code.

.NET 3.5 web application running on Windows Server 2003


Automated tool to migrate the application, making the app lighter weight because of the reduced overhead of a container vs a VM.  Increased security through what Docker provides (**what is this?**)

Traditional application are **laborious to maintain** because they have:
* No test automation - has a change broken something?
* No automated deployment - manual deployment processes that can be incorrectly documented
* No automation tests - requires the business to spend time trying to find problems, and depending on the application this could take months as it goes through financial period processes that can not be time-shift run.
* Deployment can take the whole team for a weekend to deploy and therefore is only completed 3~4 times a year

**Complex to manage** as they are:
* Special snowflakes that do things differently than all of the other snowflakes
* Custom logging
* Custom instrumentation
* Therefore you have multiple tools with manual bespoke processes to keep the application and therefore the business running.

**Expensive to run**
* Dependencies are not understood and therfore
* Run with higher isolution for safety (side effects)
* Under-utilised infrastructure, VMs running mostly not doing anything
* But this remains as it is safer than trying to conslidate the apps onto shared VMs

[Potential savings moving to Docker calculator tool](https://docker.com/roicalculator)
Benefits of docker
* Containers have a much lower overhead vs a VM
* Within a container, the app thinks its running on its own server
* The processes running the apps are on the host and therefore, no intermediate layer reducing the performance and increasing the overhead but they do provide strong isolation
* All wrapped apps have a consistent API to run/stop and get the logs
* A server that can run 10 VMs can easily run 50 containers (**what about network load?**)

## How to migrate a windows application to Docker
[Docker MTA - Part 2 video](https://www.youtube.com/watch?v=7rNTYslgJdQ&spfreload=1)

How to move a .NET 3.5 Web App running on Windows Server 2003 using the Image2Docker tool



### Image2Docker
As of writing Image2Docker was on version 1.8.3 (2017/07/12)

Image2Docker can dockerise apps on the local machine, a remote machine or by inspecting Windows Server 2003, 2008, 2012 and 2016 VM images in WIM, VHD and VHDX formats.
The PowerShell module requires Windows Server 2012 with PowerShell 5.0+ as it has a dependency on the ServerManager PowerShell module.
Can also be run on Windows 10 as long as the Windows Server 2016 Remote Server Administration Tools (RSAT) are installed.


Image2Docker can work with different types of applcations that are called artifacts, the supported ones are listed in the [README.md](https://github.com/docker/communitytools-image2docker-win/blob/master/README.md#supported-artifacts) with the current focus on ASP.NET applications running on IIS.

When working with IIS artifacts, we can pull all websites into a single Docker container however, it is best practice to extract each Web App into its own Container, to do this we pass the parameter `-ArtifactParam WebsiteName`.

For this example our ASP.NET 3.5 Web App is called SignUp.Web.


Docker2Image creates a folder with a Dockerfile and the file assets deployed to the originating IIS site.  Containers leverage the official [microsft/aspnet containers](https://hub.docker.com/r/microsoft/aspnet/).  These are based on the [microsoft/windowsservercore](https://hub.docker.com/r/microsoft/windowsservercore/) containers which contain Windows Server 2016 Server Core.  On top of this IIS 10 is enabled and is available with three versions of the .NET Framework:
* 4.7
* 4.6.2
* 3.5


On Windows 10:
1. Start PowerShell
2. `Install-Module Image2Docker`
3. `Import-Module Image2Docker`
4. You can get help on the commandlet `Get-Help ConvertTo-Dockerfile`
5. For this instance the .NET 3.5 application is installed in a VM image on our Windows 10 machine, so we can run the command
``` PowerShell
ConvertTo-Dockerfile `
-ImagePath C:\VMs\win2003-iis.vhd `
-OutputPath C:\mta
-Artifact IIS
-ArtifactParam SignUp.Web
-Verbose
```
6. Build the docker container `docker build -t newsletter/signup-app:v1 .` **or?** `docker image build -t newsletter/signup-app:v1 .`
7. Run the container `docker run -dp 8090:8080 newsletter/signup-app:v1` **or?** `docker container run --detach --publish 8090`
8. You can check the container is running and get the container ID by running `docker container ls`
9. Get the IP address of the container by running `docker inspect 498` where the container id starts 498
10. Open the site from a browser `start http://172.24.34.47:8090` based on the container running on IP 172.24.34.47


#### Image2Docker links
* [Docker blog post on Image2Docker](https://blog.docker.com/2016/09/image2docker-prototyping-windows-vm-conversions/)
* [Image2Docker Github](https://github.com/docker/communitytools-image2docker-win)
* [Image2Docker PowerShell Gallery](https://www.powershellgallery.com/packages/Image2Docker/1.8.3)

#### Windows Server 2016 Remote Server Administration Tools (RSAT) links
* [TechNet: Remote Server Administration Tools (RSAT) for Windows Client and Windows Server (dsforum2wiki)](https://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx)
* [Microsoft downloads: Remote Server Administration Tools for Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=45520)
