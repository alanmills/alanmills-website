# Installing Windows Server 2016 
**Author:** [Alan Mills]
**Date:** [02 August 2017 14:40]
**Tags:** [Windows Server 2016], [Docker], [Nano], [Window Server Core]
**Status**: Draft

With Windows Server 2016 there are three installation flavours:
* GUI
* Core
* Nano

## Core
Core starts you in the traditional dos command line and from there you can run `sconfig` the **Server Config** application to configure the server name, domain, network, activiation, etc.

You can also run `powershell` to run the **Windows PowerShell**
* `Set-DisplayResolution 1280 720` - Set the computers Display Resolution, in this case 1280x720
* `tzutil /g` - list the current timezone
* `tzutil /l` - list all of the valid timezones
* `Set-TimeZone "GMT Standard Time"` - Set the timezone to GMT Standard Time as used in UK
* `Get-NetIPAddress` - Brings back all of the interfaces
* `Get-NetIPAddress -InterfaceAlias Ethernet` - Brings back all of the interfaces on the computers physcial Ethernet port
* `New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 192.168.3.110 -PrefixLenght 24 -DefaultGateway 192.168.3.2` - Sets the IP address with a 255.255.255.0 subnet mask and Default Gateway
* `Set-NetIPInterface -InterfaceAlias Ethernet -Dhcp Enabled` - Use this version instead of the **New-NetIPAddress** if you want to use DHCP to assign an IP address to this machine.
* `Get-NetIPConfiguration -InterfaceAlias Ethernet` - Shows the Network configuration e.g. IPv4, IPv6, Default Gateway, DNSServer(s)
* `Set-NetIPConfiguration -InterfaceAlias Ethernet 
* `hostname` or `Get-Content env:computername` - Gets the computername of the machine
* `Rename-Computer -NewName server1 -Restart` - Sets the name of the computer and restarts the server for the name to take effect

* `Get-NetFirewallRule | ft` - The firewall rules on the machine
* `Get-NetFirewallRule -Name corenet-igmp-in` - When enabled allows inbound PING
* `Get-NetFirewallRule -Name corenet-igmp-out` - When enabled allows outbound PING
* `Get-NetFirewallRule -Name corenet-igmp-in | Enable-NetFirewallRule` - Enables the rule
* `Get-NetFirewallRule -Name corenet-igmp-out | Enable-NetFirewallRule` - Enables the rule

* Enable a group of firewall rules rather than just the individual rules
* `Get-NetFirewallRule | ft DisplayGroup, DisplayName` - List out the Display Groups and the DisplayName for each rule
* `Enable-NetFirewallRule -DisplayGroup "File and Printer Sharing"

** For testing purposes only in a development lab**
* `New-NetFirewallRule -DisplayName "Allow All Traffic" -Direction inbound -Action allow` - Allow all in-bound traffic
* `New-NetFirewallRule -DisplayName "Allow All Traffic" -Direction outbount -Action allow` - Allow all out-bound traffic

* `Add-Computer -DomainName "Company.pri" -Restart - Add the computer to the AD and restart the computer for the change to take effect

* `Get-WindowsFeature -ComputerName server1` - Get a list of the Windows Features available and installed on **server1**
* `Install-WindowsFeature -Name "FS-FileServer" -ComputerName server1` - Installs the **File Server** Feature on **server1**

## Remote management
From PowerShell on your remote computer
``` PowerShell
Set-Item wsman:\localhost\client\trustedhosts -value 192.168.3.112
$cred = Get-Credential 192.168.3.112\administrator
Enter-PSSession -ComputerName 192.168.3.112 -Credential $cred
```
* `` - Enter a remote PowerShell session on **server1**
