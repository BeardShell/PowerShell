# PowerShell code that everyone needs sooner or later

###### Use this document as reference for reviewing when needed

Get your PowerShell version:
```powershell
$PSVersionTable.PSVersion
```

Write your scripts and functions with strict error notifications:
```powershell
Set-StrictMode -Version Latest
```

Add support for TLS 1.2. This is often an issue when using the PowerShell Gallery:
```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```

When the PowerShell Gallery is next level broken use the following function:
```powershell
Function Reset-PowerShellGalleryStuff {
  #Sometimes everything to use the PowerShell Gallery, NuGet and all that stuff is just plain old broken.
  #To fix it there are several steps. Just run this function if necessary and try what you wanted to do again.

  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 #TLS 1.0 and 1.1 are not supported, this is usually an issue these days (source:      https://stackoverflow.com/questions/51406685/how-do-i-install-the-nuget-provider-for-powershell-on-a-unconnected-machine-so-i)
  Register-PSRepository -Default  #Same as before, this appearantly is an issue sometimes. Encountered it often. (source: https://stackoverflow.com/questions/63385304/powershell-install-no-match-was-found-for-the-specified-search-criteria-and-mo)
  Install-PackageProvider -Name NuGet #when things are working properly again, might as well install this packageprovider as well
}
```
> This function is not written by me. Credits to the original author (When I find the name of this guy, I'll edit this note)
