---
date: 2026-01-20
tags:
  - dfir
  - kape
  - forensics
  - zimmerman
---
*Kroll Artifact Parser and Extractor (KAPE) parses and extracts Windows forensics artifacts. It is a tool that can significantly reduce the time needed to respond to an incident by providing forensic artifacts from a live system or a storage device much earlier than the imaging process completes.* 

*KAPE serves two primary purposes, 1) collect files and 2) process the collected files as per the provided options. For achieving these purposes, KAPE uses the concept of targets and modules. Targets can be defined as the forensic artifacts that need to be collected. Modules are programs that process the collected artifacts and extract information from them. We will learn about them in the upcoming tasks.*

# How KAPE Works
---
KAPE processes targets in queues through two passes. The first pass, KAPE will collect what it can from files that are not OS locked. The second pass, KAPE uses a technique that uses raw disk reads to bypass OS locks and copy the files.

Once the data has been collected, KAPE can process it using modules.

# Target Options
---
A TKAPE file contains information about the artifact we want to collect (i.e. path, category, file masks).

**Compound targets** allows KAPE to collect multiple artifacts given a single command.

The **!Disabled** directory contains targets to keep in KAPE instances, but don't want to show up as active targets.

The **!Local** directory keeps artifacts local.

# Module Options
---
The MKAPE file are module files for KAPE.

The bin folder keeps a directory of tools that Windows may not have natively incase we need to use it.

# KAPE GUI
---

| Option       | Description                                                                |
| ------------ | -------------------------------------------------------------------------- |
| flush        | Deletes all content from the Target destination.                           |
| Add %d       | Appends date info to the directory name where the collected data is saved. |
| Add %m       | Append machine info to the Target destination directory.                   |
| Process VSCs | Processes Volume Shadow Copies.                                            |
| transfer     | Transfer collected artifacts through SFTP or S3 bucket.                    |
When both *use Target options* and *use Module options* are selected, a Module source is not required as it will use the Target destination as its source.

# KAPE CLI
---
When collecting targets, the switches `tsource`, `target`, and `tdest` are required.
When processing files using modules, the switches `module` and `mdest` are required.

Example:

```powershell
kape.exe --tsource C: --target KapeTriage --tdest C:\Users\Administrator\Desktop\Target --mdest C:\Users\Administrator\Desktop\module --module !EZParser
```
This must be run in an elevated shell.

# Other Notes
The `AutomaticDestination` in `FileFolderAccess` will tell you if a program was copied from a removable drive.
Check the `WordWheelQuery` to find the search query used on the system.
Use `LastKnownNetwork` to get detailed information regarding network interfaces.