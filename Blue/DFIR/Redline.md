---
date: 2026-01-19
tags:
  - dfir
  - redline
  - forensics
  - windows
  - linux
  - macos
---
*FireEye Redline will essentially give an analyst a 30,000-foot view (10 kilometers high view) of a Windows, Linux, or macOS endpoint. Using Redline, you can analyze a potentially compromised endpoint through the memory dump, including various file structures. With a nice-looking GUI (Graphical User Interface) - you can easily find the signs of malicious activities.* 

# Data Collection
---
1. **Standard Collector:** minimal amount of data for analysis. Fastest method.
2. **Comprehensive Collector:** Gathers the most data from the host. 1 hr+.
3. **IOC Search Collector (Windows Only):** Collects data that matches user created IOCs using the IOC editor. 

## Creating a Standard Collector
![../../PNG PDF JPEG/attachment-01192026-9.png](../../PNG%20PDF%20JPEG/attachment-01192026-9.png)
![../../PNG PDF JPEG/attachment-01192026-10.png](../../PNG%20PDF%20JPEG/attachment-01192026-10.png)
You can click on *Edit your script* to adjust what Redline will capture (i.e. Disks, network, memory).
![../../PNG PDF JPEG/attachment-01192026-11.png](../../PNG%20PDF%20JPEG/attachment-01192026-11.png)
Once saved to a folder, run the `RunRedlineAudit.bat` to begin collecting data from the host. The script needs to be *run as Administrator* to be able to collect the data we need.
![../../PNG PDF JPEG/attachment-01192026-12.png](../../PNG%20PDF%20JPEG/attachment-01192026-12.png)
Once completed, a *AnalysisSession.mans* file will be created and this is what we need to import into Redline for investigation.

# Analysis
---
![../../PNG PDF JPEG/attachment-01192026-13.png](../../PNG%20PDF%20JPEG/attachment-01192026-13.png)
1. **System Information**
2. **Processes**
	1. **Handle:** A connection from a process to an object/resource in Windows.
	2. **Memory Sections:** Allows for investigating unsigned memory sections.
	   *Unsigned DLLs is worth taking a closer look.*
	3. Strings: Information about captured strings.
	4. **Ports:** <mark style="background:rgba(163, 67, 31, 0.2)">Critical section to pay attention to!</mark>
	   Review suspicious connections from ports and IP addresses.
	   Pay attention to processes listed here also (i.e. notepad.exe).

# Standard Collector Analysis
---
Provide the Operating System detected for the workstation: <u>Windows Server 2019 Standard 17763</u>
![../../PNG PDF JPEG/attachment-01192026-14.png](../../PNG%20PDF%20JPEG/attachment-01192026-14.png)


What is the suspicious scheduled task that got created on the computer? <u>MSOfficeUpdateFa.ke</u>
![../../PNG PDF JPEG/attachment-01192026-15.png](../../PNG%20PDF%20JPEG/attachment-01192026-15.png)

Find the message that the intruder left for you in the task: <u>THM-p3R5IStENCe-m3Chani$m</u>
![../../PNG PDF JPEG/attachment-01192026-16.png](../../PNG%20PDF%20JPEG/attachment-01192026-16.png)

There is a new System Event ID created by an intruder with the source name "THM-Redline-User" and the Type "ERROR". Find the Event ID #: <u>546</u>
![../../PNG PDF JPEG/attachment-01192026-17.png](../../PNG%20PDF%20JPEG/attachment-01192026-17.png)
Set the Type filter to *Error* and the Source filter to *THM-Redline-User.*
![../../PNG PDF JPEG/attachment-01192026-18.png](../../PNG%20PDF%20JPEG/attachment-01192026-18.png)

Provide the message for the Event ID: <u>Someone cracked my password. Now I need to rename my puppy-++-</u>
![../../PNG PDF JPEG/attachment-01192026-19.png](../../PNG%20PDF%20JPEG/attachment-01192026-19.png)

It looks like the intruder downloaded a file containing the flag for Question 8. 
Provide the full URL of the website: <u>https://wormhole.app/download-stream/gI9vQtChjyYAmZ8Ody0AuA</u>
![../../PNG PDF JPEG/attachment-01192026-20.png](../../PNG%20PDF%20JPEG/attachment-01192026-20.png)
![../../PNG PDF JPEG/attachment-01192026-21.png](../../PNG%20PDF%20JPEG/attachment-01192026-21.png)
Confirming that this is the suspicious file downloaded: *flag.txt*

Provide the full path to where the file was downloaded to including the filename: <u>C:\Program Files (x86)\Windows Mail\SomeMailFolder\flag.txt</u>
![../../PNG PDF JPEG/attachment-01192026-22.png](../../PNG%20PDF%20JPEG/attachment-01192026-22.png)

Provide the message the intruder left for you in the file: <u>THM{600D-C@7cH-My-FR1EnD}</u>
![../../PNG PDF JPEG/attachment-01192026-23.png](../../PNG%20PDF%20JPEG/attachment-01192026-23.png)
Navigate to `C:\Program Files (x86)\Windows Mail\SomeMailFolder\` and open the flag.txt to obtain the message.

