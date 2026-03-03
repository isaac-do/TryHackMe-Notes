---
date: 2026-02-04
tags:
  - windows
  - powershell
---
# What is PowerShell
---
The Windows PowerShell was developed using Object-Oriented Programming and it processes **objects**. *Objects* represent an item with properties (characteristics) and methods (actions).

# PowerShell Basics
---
**Cmdlets** follow the `Verb-Noun` naming convention. For example, Get-Content.

# Real-Time System Analysis
---
Get-FileHash cmdlet generates file hashes.
Example:
```powershell
Get-FileHash -Path .\ship-flag.txt
```

View Alternate Data Streams (ADS) to uncover hidden stream attached to a file.
Example:
```powershell
Get-Item -Path "C:\House\house_log.txt" -Stream *
```


# Remote Execution
---
The `Invoke-Command` executes commands on remote syststems.
Example (run a script on a server):
```powershell
Invoke-Command - FilePath C:\scripts\test.ps1 -ComputerName Server01
```


Example (run a command on a remote server):
```powershell
Invoke-Command -ComputerName Server01 -Credential Domain01\User01 -ScriptBlock {Get-Service}
```
