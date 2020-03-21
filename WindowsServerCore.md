Get-WindowsFeatures 
Get-WindowsOptionalFeature -online | ft


Install-WindowsFeature -Name NET-Framework-Features -IncludeAllSubFeature
Install-WindowsFeature -Name NFS-Client -IncludeAllSubFeature
Install-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature
Install-WindowsFeature -Name Remote-Desktop-Services -IncludeAllSubFeature

Enable-WindowsOptionalFeature -Online -FeatureName Containers -All
