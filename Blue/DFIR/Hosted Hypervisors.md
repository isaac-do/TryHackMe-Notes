---
date: 2026-01-28
tags:
  - dfir
  - forensics
  - hypervisors
  - vmware
  - virtualbox
---
# VirtualBox Logs
---
VirtualBox's command line tool `VBoxManage.exe` is located in `C:\Program Files\Oracle\VirtualBox`.

| Log Name           | Description                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| VirtualBox.xml     | While this is not strictly a log file but rather a configuration file used by VirtualBox, we can get good information from it, like the interface IP used by the Hypervisor, the virtual interfaces used by a VM, uid, log locations, VM files location, etc. We need to be careful not to edit this file since the Hypervisors use it, and it could lead to failure if settings are not properly set. |
| selectorwindow.log | contains information about the VirtualBox process (usually the details used to install/start).                                                                                                                                                                                                                                                                                                         |
| VboxSVC            | saves information regarding the start/stop use of VirtualBox and also saves/deletes components.                                                                                                                                                                                                                                                                                                        |
| Vbox.log           | This file contains information such as the OS, with details about their architecture and installation dates (timestamps). It also contains information about the VM plugins and drivers that were implemented (audio, USB, etc.).                                                                                                                                                                      |
| VBoxHardening.log  | This file will provide us with information about the hardening performed on VirtualBox; this is focused on the memory management of the Hypervisor. While this information can be hard to read and is not well documented,  if we were looking for some memory manipulation (like an escapeVM attack), we could detect it here. Also, some security-related crashes will be visible in this file.      |
# VMware Logs
---
Settings and configuration information: `C:\ProgramData\VMware\VMware Workstation`.

| Log Name        | Description                                                                                                             |
| --------------- | ----------------------------------------------------------------------------------------------------------------------- |
| settings.ini    | This file contains configuration settings, such as printers and disks, which are applied globally across different VMs. |
| config.ini      | Contains specific configurations like Auto-Update, access ports, and others.                                            |
| vmautostart.xml | Contains configuration information on how Virtual Machines are supposed to start.                                       |
Hypervisor logs in Windows: `C:\ProgramData\VMware\logs`.

## Memory Dump
Take a snapshot of the VM then use the tool vmss2core.
```powershell
vmss2core -W {snampshot.vmss} {new name of dump.vmem}
```