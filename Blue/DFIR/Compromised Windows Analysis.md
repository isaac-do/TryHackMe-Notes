---
date: 2026-01-22
tags:
  - dfir
  - windows
  - forensics
  - lecmd
  - timelineexplorer
  - zimmerman
---
# Timeline Explorer for Visualization
---
***Timeline Explorer** is a tool developed by Eric Zimmerman that allows you to view CSV files easily.*
# Investigating Persistence
Threat actors often use the **Windows Task Scheduler** to create a backdoor to stay persistent. As a forensic investigator, this is a crucial artifact while analyzing compromised Windows OS.

An alternative way to view Scheduled tasks is traverse the directory `C:\Windows\System32\Tasks`.

# Investigating Recently Accessed Files
---
***LNK files** are shortcuts to the original files. LNK files give information on the recently accessed files/folders.* 

These files serve as valuable artifact while searching for the attacker's footprints.

Recent files location:
`C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Recent Items`

## Parsing LNK Files
Eric Zimmeran's tool, **LECmd**, parses LNK files.

Example usage of this tool:
```powershell
.\LECmd.exe -d C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Recent --csvf Parsed-LNK.csv --csv C:\Users\Administrator\Desktop
```

# Investigating File Execution
---
## Prefetch Files
*Prefetch files are part of the Windows OS that keeps track of your file execution activities.*
*When you execute a file, Windows create a prefetch file for it and remembers how it was opened and what activites it performed. This helps OS load these programs faster the next time you open it.*

Useful for forensic investigators in finding details of any suspicious or malicious programs executed.

## Parsing Prefetch Files
Eric Zimmerman's tool, PECmd, parses prefetch files.

Example usage of this tool:
```powershell
.\PECmd.exe -d "C:\Windows\Prefetch" --csv C:\Users\Administrator\Desktop --csvf Prefetch-Parsed.csv
```

# The Dig of Executable
---
## Amcache File
*Amcache files stores applications metadata to ensure their compatibility with the Windows environment.*

Using this artifiact, forensic investigators can extract the file metadata, including the file hash, execution time, and other details.

## Parsing Amcache
Eric Zimmerman's tool, Amcache Parser, parses Amcache files.

Example usage of this tool:
```powershell
.\AmcacheParser.exe -f "C:\Windows\appcompat\Programs\Amcache.hve" --csv C:\Users\Administrator\Desktop --csvf Amcache_Parsed.csv
```
**Important Note:** The Amcache refreshes itself after every restart.

# Windows Event Log Analysis
---
To check for RDP connections, go to:
`Applications and Services Logs -> Microsoft -> Windows -> Terminal-Services-RemoteConnectionManager > Operational`
Find Event ID `1149` which indicates successful RDP login.

To check when Windows Defender was turned off, go to:
`Applications and Services Logs > Microsoft > Windows > Windows Defender > Operational`
Find Event ID `5001` which indicates that Windows Defender was turned off.

# Extra Resources
- See [Windows Forensics 1](Windows%20Forensics%201.md) for related Windows forensics regarding the registry.