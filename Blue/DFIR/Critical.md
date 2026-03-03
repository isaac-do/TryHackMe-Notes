---
date: 2026-01-25
tags:
  - dfir
  - memory
  - windows
  - volatility3
  - forensics
---
Relevant Volatility3 Plugins:

| Name                 | Description                                                                |
| -------------------- | -------------------------------------------------------------------------- |
| Windows.cmdline      | List process command line arguments.                                       |
| Windows.drivermodule | Determines if any loaded drivers were hidden by a rootkit.                 |
| Windows.filescan     | Scans for file objects present in a particular Windows memory image.       |
| Windows.getids       | Print the SIDs owning each process.                                        |
| Windows.handles      | List process open handles.                                                 |
| Windows.info         | Shows OS & kernel details of the memory sample being analyzed.             |
| Windows.netscan      | Scans for network objects present in a particular Windows memory image.    |
| Windows.netstat      | Traverses network structures present in a particular Windows memory image. |
| Windows.mftscan      | Scans for Alternate Data Stream.                                           |
| Windows.plist        | Lists the processes present in a particular Windows memory image.          |
| Windows.pstree       | List processes in a tree based on their parent process ID.                 |

# Obtaining Information
---
Use the `-f` switch with the `windows.info` plugin to gather general information:
```bash
vol -f memdump.mem windows.info
```
![attachment-01262026.png](../../PNG%20PDF%20JPEG/attachment-01262026.png)

# Examine Memory
---
To dump the memory region corresponding to malicious.exe, run the command:
```bash
vol -f memdump.mem -o . windows.memmap --dump --pid 1612
```
- The -o switch specifies the output directory