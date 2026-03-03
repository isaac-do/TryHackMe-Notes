---
date: 2026-01-19
tags:
  - registry
  - windows
  - registryexplorer
  - dfir
---
# Windows Registry
---
The Windows registry is a database of windows configuration data. View using `regedit.exe`

| Root Key                   | Description                                                                          |
| -------------------------- | ------------------------------------------------------------------------------------ |
| HKEY_CURRENT_USER (HKCU)   | Contains current logged in user's information (folders, wallpaper).                  |
| HKEY_USERS (HKU)           | Contains all actively loaded user profiles.                                          |
| HKEY_LOCAL_MACHINE (HKLM)  | Contains configuration information for the local machine.                            |
| HKEY_CLASSES_ROOTS (HKCR)  | This makes sure the correct program opens when a file is opened using file explorer. |
| HKEY_CURRENT_CONFIG (HKCC) | Contains information about the hardware profile of the local machine.                |

# Accessing Registry Hives Offline
---
Where to find registry files for **disk images:** `C:\Windows\System32\Config`

## User Data Hives
Hives containing user data are found in User profile directories: `C:\Users\<username>\`
*These files are hidden*

| File Name    | Description                          | Location                                              |
| ------------ | ------------------------------------ | ----------------------------------------------------- |
| NTUSER.DAT   | Mounted on HKCU when a user logs in. | C:\Users\\\<username>\                                |
| USRCLASS.DAT | Mounted on HKCU\Software\Classes.    | C:\Users\\\<username>\AppData\Local\Microsoft\Windows |
## Amcache Hive
These hives tore information about programs that were recently ran on Windows.
Location: `C:\Windows\AppCompat\Programs\Amcache.hve`

## Transaction Logs and Backups
**Transaction logs** keep a journal of the changelog in the registry hive. They may have the most recent changes that haven't yet been reflected in the registry hives.
Location: `C:\Windows\System32\Config`

**Registry backups** registry hives every 10 days. 
Hives are copied to `C:\Windows\System32\Config\RegBack`
*Check here if we suspect registry keys that may have been deleted or modified recently*

# System Information and System Accounts
---
Control Sets are machine configuration data used for controlling the system startup. 

| Control Set       | Description                                                      | Location                      |
| ----------------- | ---------------------------------------------------------------- | ----------------------------- |
| ControlSet001     | the Control Set the machine booted with.                         | SYSTEM\ControlSet001          |
| ControlSet002     | last known good configuration.                                   | SYSTEM\ControlSet002          |
| CurrentControlSet | <u>Volatile</u> and stores the most accurate system information. | HKLM\SYSTEM\CurrentControlSet |
## Autostart Programs (Autoruns)
Includes information about programs or commands that run when a user logs on.

## SAM hive and user information
Contains user account information, login information, and group information.

# Usage or knowledge of files/folders
---
## Shellbangs
These store user specific information when they open a folder like specific layouts. Can identify the Most Recently Used files and folders.

## Open/Save and LastVisited Dialog MRU
When we open/save a file, a dialog box appears asking us where to save or open that file from. Windows remembers the location of the opened/saved file.

# Evidence of Execution
---
## UserAssist
Windows keeps track of applications launched by the user using Windows explorer.

## ShimCache
This is also called the Application Compatibility Cache (AppCompatCache). It is used to keep track of application compatibility with the OS and tracks all applications launched on the machine.

## AmCache
Performs similar function to the ShimCache but stores additional information regarding the application execution.

## BAM/DAM
Background Activity Monitoring (BAM) keeps a tab on the activity of background applications.
Desktop Activity Monitor (DAM) is part of Windows that optimizes power consumption of the device.

# Challenge
---
*One of the Desktops in the research lab at Organization X is suspected to have been accessed by someone unauthorized. Although they generally have only one user account per Desktop, there were multiple user accounts observed on this system. It is also suspected that the system was connected to some network drive, and a USB device was connected to the system. The triage data from the system was collected and placed on the attached VM. Can you help Organization X with finding answers to the below questions?*

*Note: When loading registry hives in RegistryExplorer, it will caution us that the hives are dirty. This is nothing to be afraid of. We just need to remember the little lesson about transaction logs and point RegistryExplorer to the .LOG1 and .LOG2 files with the same filename as the registry hive. It will automatically integrate the transaction logs and create a 'clean' hive. Once we tell RegistryExplorer where to save the clean hive, we can use that for our analysis and we won't need to load the dirty hives anymore. RegistryExplorer will guide you through this process.* 

How many user created accounts are present on the system? <u>3</u>
![../../PNG PDF JPEG/attachment-01192026.png](../../PNG%20PDF%20JPEG/attachment-01192026.png)
![../../PNG PDF JPEG/attachment-01192026-1.png](../../PNG%20PDF%20JPEG/attachment-01192026-1.png)
![../../PNG PDF JPEG/attachment-01192026-2.png](../../PNG%20PDF%20JPEG/attachment-01192026-2.png)
Navigate to `SAM\Domains\Account\Users`. Users with RID beginning 10xx are user created accounts.

What is the username of the account that has never been logged in? <u>thm-user2</u>
![../../PNG PDF JPEG/attachment-01192026-3.png](../../PNG%20PDF%20JPEG/attachment-01192026-3.png)

What's the password hint for the user THM-4n6? <u>count</u>
![../../PNG PDF JPEG/attachment-01192026-4.png](../../PNG%20PDF%20JPEG/attachment-01192026-4.png)

When was the file 'Changelog.txt' accessed? <u>2021-11-24 18:18:48</u>
![../../PNG PDF JPEG/attachment-01192026-5.png](../../PNG%20PDF%20JPEG/attachment-01192026-5.png)
![../../PNG PDF JPEG/attachment-01192026-6.png](../../PNG%20PDF%20JPEG/attachment-01192026-6.png)
To find recent docs, navigate to *NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs*.

What is the complete path from where the python 3.8.2 installer was run? <u>Z:\setups\python-3.8.2.exe</u>
![attachment-01192026-7](../../PNG%20PDF%20JPEG/attachment-01192026-7.png)
To find applications launched by the user, navigate to `NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count`.

When was the USB device with the friendly name 'USB' last connected? <u>2021-11-24 18:40:06</u>
![../../PNG PDF JPEG/attachment-01192026-8.png](../../PNG%20PDF%20JPEG/attachment-01192026-8.png)
In the top left RegistryExplorer, navigate to `SYSTEM\CurrentControlSet\Enum\USBSTOR` to see all tracked USB devices plugged into the system.

In the bottom right RegistryExplorer, navigate to `SOFTWARE\Microsoft\Windows Portable Devices\Devices` to obtain the friendly device name and its associated GUID. Then look in the top left RegistryExplorer and find the corresponding GUID and obtain the `last connected date`.

# Extra Resources
- See  [Forensics - Registry Furensics](Forensics%20-%20Registry%20Furensics.md) for further explanations regarding Windows registry and common Registry Hives for forensic investigations.
- See [Compromised Windows Analysis](Compromised%20Windows%20Analysis.md) for more information about investigating file executions, recently accessed files, and task scheduler.

![../../PNG PDF JPEG/attachment-01192026.pdf](../../PNG%20PDF%20JPEG/attachment-01192026.pdf)