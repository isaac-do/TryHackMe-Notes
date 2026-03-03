---
date: 2026-01-21
tags:
  - dfir
  - forensics
  - windows
  - registry
  - registryexplorer
---
*The **Windows registry** contains all the information that the Windows OS needs for its functioning.*

The several separate files each storing information on different configuration settings are called **Hives.**


| Hive Name    | Contains                                                                                         | Location                                                       |
| ------------ | ------------------------------------------------------------------------------------------------ | -------------------------------------------------------------- |
| SYSTEM       | - Services<br>- Mounted Devices<br>- Boot Conf<br>- Drivers<br>- Hardware                        | C:\Windows\System32\config\SYSTEM                              |
| SECURITY     | - Local Security Policies<br>- Audit Policy Settings                                             | C:\Windows\System32\config\SECURITY                            |
| SOFTWARE     | - Installed Programs<br>- OS Version and other info<br>- Autostarts<br>- Program Settings        | C:\Windows\System32\config\SOFTWARE                            |
| SAM          | - Usernames and their Metadata<br>- Password Hashes<br>- Group Memberships<br>- Account Statuses | C:\Windows\System32\config\SAM                                 |
| NTUSER.DAT   | - Recent Files<br>- User Pref<br>- User-specific Autostarts                                      | C:\Users\username\NTUSER,DAT<br>*Hidden file*                  |
| USRCLASS.DAT | - Shellbangs<br>- Jump Lists                                                                     | C:\Users\username\AppData\Local\Microsoft\Windows\USRCLASS.DAT |
**Note:** The configuration settings stored in each hives are some examples. These are not all the configurations that each hives store.

*HKCR and HKCC are dynamically populatd when Windows is running.*

# Registry Forensics
---
The table below lists some registry keys that are useful during forensic investigations.

| Registry Key                                                           | Importance                                                                                    |
| ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist     | Stores information on recently accessed applications launched via the GUI.                    |
| HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths     | Stores all the paths and locations typed by the user inside the Explorer address bar.         |
| HKLM\Software\Microsoft\Windows\CurrentVersion\App Paths               | Stores the path of the applications.                                                          |
| HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery | Stores all the serach terms typed by the user in the Explorer search bar.                     |
| HKLM\Software\Microsoft\Windows\CurrentVersion\Run                     | Stores information on the programs that are set to automatically start when the user logs in. |
| HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs     | Stores information on the files that the user has recently accessed.                          |
| HKLM\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName        | Stores the computer's name (hostname).                                                        |
| HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall               | Stores information on installed programs.                                                     |

## Handling Dirty Hives
To handle *dirty* hives in RegistryExplorer, **hold SHIFT** and press **Open** when selecting the desired hive file.

# Extra Resources
- See [Windows Forensics 1](Windows%20Forensics%201.md) for a registry cheatsheet and related resources regarding Windows registry forensic analysis.