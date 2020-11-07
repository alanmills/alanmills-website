# Installing Docker on Windows Server 2016 
**Author:** [Alan Mills]
**Date:** [02 August 2017 20:34]
**Tags:** [Windows Server 2016], [Docker], [Nano], [Window Server Core]
**Status**: Draft

On Windows Server 2016
1. Login as an Administrator
2. Update windows, Run **PowerShell** as an **Administrator**
3. `sconfig`, option 6, select **A**ll and install **A**ll
4. Once all the updates have been applied and Windows has rebooted (a few times?)
5. Run **PowerShell** as an **Administrator** 
6. Install the **Docker-Microsoft Provider** `Install-Module -Name DockerMsftProvider -Repository PSGallery -Force`
7. Install **Docker** `Install-Package -Name docker -ProviderName DockerMsftProvider
8. Restart Windows (of course...) `restart /r /t 1 /d p:2:4`
9. Log back in the Administrator and run **PowerShell** as an **Administrator** 
10. Check that the docker service is installed and running `Get-Service docker`
11. Run `docker version` to validate that the docker engine is running
