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
- https://stackoverflow.com/questions/46684230/visualstudio-build-tools-2017-offline-installer
- https://gist.github.com/Johlandabee/1387485124a678b9b3c8497090f58a96



## Windows Admin Center

git openssl must be on path

```
openssl req -x509 -newkey rsa:4096 -sha256 -days 365 -nodes -keyout win-8irmjn1es50.key -out win-8irmjn1es50.crt -subj "/CN=win-8irmjn1es50" -addext "subjectAltName=DNS:win-8irmjn1es50" -addext "extendedKeyUsage=serverAuth,clientAuth,codeSigning"
  
openssl pkcs12 -export -out win-d2fuota0m14.pfx -inkey 'C:\Users\john\Downloads\win-d2fuota0m14.key' -in 'C:\Users\john\Downloads\win-d2fuota0m14.crt'

$credential = Get-Credential -UserName 'Enter password below' -Message 'Enter password below'
Import-PfxCertificate -FilePath win-d2fuota0m14.pfx -CertStoreLocation Cert:\LocalMachine\My -Password $credential.Password

winrm create winrm/config/listener?Address=IP:xxxx+Transport=HTTPS @{Hostname="win-8irmjn1es50";CertificateThumbprint="";Port="5986"}

New-NetFirewallRule -DisplayName "Windows Remote Management (HTTPS-In)" -Name "Windows Remote Management (HTTPS-In)" -Profile Any -LocalPort 5986 -Protocol TCP

```
 

