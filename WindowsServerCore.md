```
Get-WindowsFeature
Get-WindowsOptionalFeature -online | ft


Install-WindowsFeature -Name NET-Framework-Features -IncludeAllSubFeature
Install-WindowsFeature -Name NFS-Client -IncludeAllSubFeature
Install-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature
Install-WindowsFeature -Name Remote-Desktop-Services -IncludeAllSubFeature

Enable-WindowsOptionalFeature -Online -FeatureName Containers -All

Invoke-WebRequest "https://aka.ms/vs/16/release/vs_BuildTools.exe" -OutFile vs_BuildTools.exe -UseBasicParsing 

$Params = "--add Microsoft.VisualStudio.Workload.MSBuildTools `
--add Microsoft.VisualStudio.Workload.WebBuildTools `
--add Microsoft.Net.Component.4.7.SDK `
--add Microsoft.Net.Component.4.7.TargetingPack `
--add Microsoft.Net.ComponentGroup.4.7.DeveloperTools `
--norestart --locale en-US
--quiet --wait"

Start-Process -FilePath 'vs_BuildTools.exe' -ArgumentList $Params -Wait 

Remove-Item .\vs_BuildTools.exe 
  
setx /M PATH $($Env:PATH + ';' + ${Env:ProgramFiles(x86)} + '\Microsoft Visual Studio\2019\BuildTools\MSBuild\15.0\Bin')


```
Sources:

- https://gist.github.com/Johlandabee/1387485124a678b9b3c8497090f58a96
