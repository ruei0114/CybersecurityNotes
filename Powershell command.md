> [!info] LINK & TAG
> [TryHackMe | Windows PowerShell](https://tryhackme.com/room/windowspowershell)
> 
> #ctf #TryHackMe #cybersecurity #Windows #shell

> [!example] Table of contents
> 1. [[Powershell command#<font color = "8080c0">GENERAL</font>| GENERAL]]
> 	- [[#Get-Command]]
> 	-  [[#Get-Help]]
> 	- [[#Get-Alias]]
> 	- [[#Find-Module]]
> 	- [[#Install-Module]]
> 2. [[#<font color = "8080c0">FILE SYSTEM</font>|FILE SYSTEM]]
> 	- [[#Get-ChildItem]]
> 	- [[#Set-Location]]
> 	- [[#New-Item]]
> 	- [[#Remove-Item]]
> 	- [[#Get-Content]]
> 3. [[#<font color = "8080c0">PIPING, FILTERING AND SORTING DATA</font>|PIPING, FILTERING AND SORTING DATA]]
> 	- [[#Piping (& Sorting)]]
> 	- [[#Filtering]]
> 		- [[#comparison operator]]
> 		- [[#-like]]
> 		- [[#Select-Object]]
> 		- [[#Select-String]]
>  4. [[#<font color = "8080c0">SYSTEM AND NETWORK INFORMATION</font>|SYSTEM AND NETWORK INFORMATION]]
> 	- [[#Get-ComputerInfo]]
> 	- [[#Get-LocalUser]]
> 	- [[#Get-NetIPConfiguration]]
> 	- [[#Get-NetIPAddress]]


## <font color = "8080c0">GENERAL</font>

### Get-Command
- <font color = "c2e3f4">discovering what commands one can use</font>
```powershell
PS C:\Users\captain> Get-Command 
CommandType Name Version Source 
----------- ---- ------- ------ 
Alias Add-AppPackage 2.0.1.0 Appx 
Alias Add-AppPackageVolume 2.0.1.0 Appx 
Alias Add-AppProvisionedPackage 3.0 Dism 
[...] 
Function A: 
Function Add-BCDataCacheExtension 1.0.0.0 BranchCache 
Function Add-DnsClientDohServerAddress 1.0.0.0 DnsClient 
[...] 
Cmdlet Add-AppxPackage 2.0.1.0 Appx 
Cmdlet Add-AppxProvisionedPackage 3.0 Dism 
Cmdlet Add-AppxVolume 2.0.1.0 Appx 
[...]
```
- <font color = "c2e3f4">display only the available commands of type</font>
	-  <font color = "efb730">Get-Command+[-CommandInfo]+["Info"]</font>
```powershell
PS C:\Users\captain> Get-Command -CommandType "Function" 
CommandType Name Version Source 
----------- ---- ------- ------ 
Function A: Function Add-BCDataCacheExtension 1.0.0.0 BranchCache Function Add-DnsClientDohServerAddress 1.0.0.0 DnsClient 
Function Add-DnsClientNrptRule 1.0.0.0 DnsClient 
[...]
```

---

### Get-Help
- <font color = "c2e3f4">provides detailed information about cmdlets</font>
	-  <font color = "efb730">Get-Help+[Command]</font>
```powershell
PS C:\Users\captain> Get-Help Get-Date 
NAME 
	Get-Date 
SYNOPSIS 
	Gets the current date and time. 
SYNTAX 
	Get-Date [[-Date] <System.DateTime>] 
	[...]
```
- <font color = "c2e3f4"> show a list of common ways in which the chosen cmdlet can be used</font>
	- <font color = "efb730">Get-Help+[Command]+[-examples]</font>
	
---

### Get-Alias
- <font color = "c2e3f4">provides shortcuts or alternative names for cmdlets</font>
	- <font color = "efb730">Get-Alias</font>
	-  <font color = "efb730">Get-Alias+[Command]</font>
```powershell
PS C:\Users\captain> Get-Alias echo  

CommandType     Name                        Version    Source   
-----------     ----                        -------    ------    
Alias           echo -> Write-Output


```

---

### Find-Module
- <font color = "c2e3f4">To search for modules (collections of cmdlets) in online repositories like the PowerShell Gallery</font>
	-  <font color = "efb730">Find-Module+[-filter]+["ModuleName"]</font>
```powershell
PS C:\Users\captain> Find-Module -Name "PowerShell*" 

Version Name Repository Description 
------- ---- ---------- ----------- 
0.4.7 powershell-yaml PSGallery Powershell module for serializing and deserializing YAML 

2.2.5 PowerShellGet PSGallery PowerShell module with commands for discovering, installing, updating and publishing the PowerShell artifacts like Modules, DSC Resources, Role Capabilities and Scripts. 
1.0.80.0 PowerShell.Module.InvokeWinGet PSGallery Module to Invoke WinGet and parse the output in PSOjects 

0.17.0 PowerShellForGitHub PSGallery PowerShell wrapper for GitHub API
```

---

### Install-Module
- <font color = "c2e3f4">download and install module from the repository</font>
	-  <font color = "efb730">Install-Module+[-filter]+["ModuleName"]</font>

---

## <font color = "8080c0">FILE SYSTEM</font>

### Get-ChildItem
- <font color = "c2e3f4">Alias as </font> `dir`
- <font color = "c2e3f4">provides detailed information about cmdlets</font>
```powershell
PS C:\Users\captain> Get-ChildItem

    Directory: C:\Users\captain

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-r---          5/8/2021   9:15 AM                Desktop
d-r---          9/4/2024  11:48 AM                Documents
d-r---          5/8/2021   9:15 AM                Downloads
d-r---          5/8/2021   9:15 AM                Favorites
d-r---          5/8/2021   9:15 AM                Links
d-r---          5/8/2021   9:15 AM                Music
d-r---          5/8/2021   9:15 AM                Pictures
d-----          5/8/2021   9:15 AM                Saved Games
d-r---          5/8/2021   9:15 AM                Videos

```

---

### Set-Location
- <font color = "c2e3f4">Alias as </font>`cd`
- <font color = "c2e3f4">navigate to a different directory</font>
	-  <font color = "efb730">Set-Location+[-Path]+[".\path"]</font>
```
PS C:\Users\captain> Set-Location -Path ".\Documents" 
PS C:\Users\captain\Documents>
```

---

### New-Item
- <font color = "c2e3f4">create an item</font>
	-  <font color = "efb730">New-Item+[-Path]+["path"]+[-ItemType]+["ItemType"]</font>
```powershell
PS C:\Users\captain\Documents> New-Item -Path ".\captain-cabin\captain-wardrobe" -ItemType "Directory" 
	
	Directory: C:\Users\captain\Documents\captain-cabin 
	
Mode LastWriteTime Length Name 
---- ------------- ------ ---- 
d----- 9/4/2024 12:20 PM captain-wardrobe 

PS C:\Users\captain\Documents> New-Item -Path ".\captain-cabin\captain-wardrobe\captain-boots.txt" -ItemType "File" 

	Directory: C:\Users\captain\Documents\captain-cabin\captain-wardrobe 
	
Mode LastWriteTime Length Name 
---- ------------- ------ ---- 
-a---- 9/4/2024 11:46 AM 0 captain-boots.txt
```

---

### Remove-Item
- <font color = "c2e3f4">remove an item</font>
	-  <font color = "efb730"> Remove-Item+[-Path]+["path"]</font>

---

### Get-Content
- <font color = "c2e3f4">Alias as</font>`type`
- <font color = "efb730"> Get-Content+[-Path]+["path"]</font>

---

## <font color = "8080c0">PIPING, FILTERING AND SORTING DATA</font>

### Piping (& Sorting)
- <font color = "c2e3f4">allows the output of one command to be used as the input for another</font>
- <font color = "efb730">command1+[|]+command2</font>
```powershell
PS C:\Users\captain> Get-ChildItem | Sort-Object Length


    Directory: C:\Users\captain


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-r---          5/8/2021   9:15 AM                Pictures
d-r---          5/8/2021   9:15 AM                Music
d-r---          5/8/2021   9:15 AM                Videos
d-----          5/8/2021   9:15 AM                Saved Games
d-r---          5/8/2021   9:15 AM                Links
d-r---          9/4/2024  11:48 AM                Documents
d-r---          5/8/2021   9:15 AM                Desktop
d-r---          5/8/2021   9:15 AM                Favorites
d-r---          5/8/2021   9:15 AM                Downloads

```

---

### Filtering
#### comparison operator

- <font color = "c2e3f4">based on specified conditions, returning only those that meet the criteria</font>
- <font color = "efb730">Get-ChildItem+[|]+Where-Object+[-Property]+["property"]+[comparison operator]+["target"]</font>

```powershell
PS C:\Users\captain\Documents\captain-cabin> Get-ChildItem | Where-Object -Property "Extension" -eq ".txt" 

	Directory: C:\Users\captain\Documents\captain-cabin Mode 
	
LastWriteTime Length Name 
---- ------------- ------ ---- 
-a---- 9/4/2024 12:50 PM 0 captain-boots.txt 
-a---- 9/4/2024 12:14 PM 264 captain-hat.txt 
-a---- 9/4/2024 12:14 PM 264 captain-hat2.txt 
-a---- 9/4/2024 12:37 PM 2116 ship-flag.txt
```
	
- `eq` : equal
- `ne` : not equal
- `gt` : greater than
- `ge` : greater than or equal
- `lt` : less than
- `le` : less than or equal

---

#### -like

- <font color = "c2e3f4">match a specific pattern</font>
- <font color = "efb730">Get-ChildItem+[|]+Where-Object+[-Property]+["property"]+[-like]+["target"]</font>

```powershell
PS C:\Users\captain\Documents\captain-cabin> Get-ChildItem | Where-Object -Property "Name" -like "ship*" 

	Directory: C:\Users\captain\Documents\captain-cabin 

Mode LastWriteTime Length Name 
---- ------------- ------ ---- 
-a---- 9/4/2024 12:37 PM 2116 ship-flag.txt
```
	
---

#### Select-Object

- <font color = "c2e3f4">match a specific pattern</font>
- <font color = "efb730">Get-ChildItem+[|]+Where-Object+[-Property]+["property"]+[-like]+["target"]</font>

```powershell
PS C:\Users\captain\Documents\captain-cabin> Get-ChildItem | Where-Object -Property "Name" -like "ship*" 

	Directory: C:\Users\captain\Documents\captain-cabin 

Mode LastWriteTime Length Name 
---- ------------- ------ ---- 
-a---- 9/4/2024 12:37 PM 2116 ship-flag.txt
```
	
---

#### Select-String

- <font color = "c2e3f4">find string</font>

```powershell
PS C:\Users\captain\Documents\captain-cabin> Select-String -Path ".\captain-hat.txt" -Pattern "hat" 

captain-hat.txt:8:Don't touch my hat!
```
---

## <font color = "8080c0">SYSTEM AND NETWORK INFORMATION</font>

### Get-ComputerInfo
- <font color = "c2e3f4">get computer info</font>

---

### Get-LocalUser
- <font color = "c2e3f4">lists all the local user accounts on the system.</font>

---

### Get-NetIPConfiguration
- <font color = "c2e3f4">provides detailed information about the network interfaces on the system, including IP addresses, DNS servers, and gateway configurations.</font>

---

### Get-NetIPAddress
- <font color = "c2e3f4">show details for all IP addresses configured on the system, including those that are not currently active</font>

---

