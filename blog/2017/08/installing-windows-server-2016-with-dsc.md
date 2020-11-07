# Installing Windows Server 2016 with DSC 
**Author:** [Alan Mills]
**Date:** [02 August 2017 18:07]
**Tags:** [Windows Server 2016], [Docker], [Nano], [Window Server Core], [DSC]
**Status**: Draft

DSC configuration file **server2.ps1**
``` PowerShell
Configuration Server2
{
  Node "server2" {
    WindowsFeature DHCPServer {
      Ensure = "Present"
      Name = "DHCP
    }
  }
}

server2
Start-DscConfiguration -Wait -Force -Path .\Server2 -Verbose
```

1. Copy **server2.ps1** to server2
2. On Server2, start PowerShell and navigate to the folder with the **server2.ps1** file
3. Run the command `.\server2.ps1`

## Preparing a Server for DSC
``` PowerShell
Get-PackageSource -Name PSGallery | Set-PackageSource -Trusted -Force -ForceBootstrap

Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force

Install-Module xComputerManagement -Force
Install-Module xNetworking -Force

Write-Host "You man now execute '.\configureServer.ps1`
```

PSGallery is installed by default
