---
date: 2026-01-27
tags:
  - dfir
  - forensics
  - ftk-imager
---
# Case B4DM755 - Details
---
Suspect:

    William S. McClean (William Super McClean)

Nationality:

    British

Charges Pressed / Accused Crimes:

    Corporate espionage
    Theft of trade secrets


*As a Forensics Lab Analyst, you analyse the artefacts from crime scenes. Occasionally, the law enforcement agency you work for receives "intelligence reports" about different cases, and today is one such day. A trusted informant, who has connections to an international crime syndicate, contacted your supervisor about William S. McClean from Case#B4DM755.*

*The informant provided information about the suspect's whereabouts in Metro Manila, Philippines, which is currently at large, and a transaction that will happen today with a local gang member. They also knew the exact location of the meetup and that the suspect would have incriminating materials at the time.*

*The law enforcement agency prepared for the operation by obtaining proper search authority and assigning a DFIR (Digital Forensics & Incident Response) First Responder (i.e., you) to ensure the appropriate acquisition of digital artefacts and evidence for examination at the Forensics Lab, and eventually for use in litigation. The court issued a search warrant on the same day, allowing law enforcement officers to investigate the suspect and his place of residence based on the informant's tip.* 

# Practical Application of the Digital Forensics Process
---
## Forensic Acquisition Process for Digital Artefacts and Evidence
 DFIR First Responders should typically adhere to the following guidelines if there is any computer system at the scene of a crime:
 - Taking an image of the RAM.
 - Checking for **drive encryption**.
 - Taking an image of the drive(s).


## Process for Establishing Chain of Custody 
---
DFIR First Responders should typically adhere to the following guidelines when handling digital artefacts and evidence before, during, and after collection:
- Ensure **proper documentation** of any seized materials as evidence (devices/files).
- **Hash and copy** obtained files to maintain the integrity of the original.
- Do not perform an appropriate shutdown of devices. <u>Pull the power plug from suspect devices instead</u>. This is to avoid data alteration as a proper shutdown may trigger anti-forensic measures.
- **Bag, Seal, and Tag the obtained artefacts** before sending them to the Forensics Laboratory.

# Case B4DM755: At the Scene of Crime
---
*Unfortunately, law enforcement arrived late at the suspect's residence, where the transaction supposedly happened. Upon arrival, everyone appeared to have already left; there were indications of evidence eradication attempts, and a transaction between the nefarious elements had successfully occurred.* 

*During a thorough search of the suspect's place, law enforcement officers discovered a flash drive under the desk. A key chain with the initials WSM was attached to the flash drive, which the team believes belongs to the suspect. It may have been left behind accidentally in their haste to vacate the place.* 

 *As a DFIR First Responder accompanying the Field Operatives, you documented, labelled, and preserved the artefact found and completed the Chain of Custody form. You then transported the artefact to the Forensics Laboratory for further examination.* 

What is the only possible artefact found in the suspect's residence? 
Answer: **flash drive**
Based on the scenario, what should be done with that acquired suspect artefact? 
Answer: **taking an image**

# Introduction to FTK Imager
---
*FTK Imager is a forensics tool that allows forensic specialists to acquire computer data and perform analysis without affecting the original evidence, preserving its authenticity, integrity, and validity for presentation during a trial in a court of law.* 

**NOTE:** In a real-world scenario, a Forensics Lab Analyst will use a write-blocking device to mount the suspect drive / forensic artefact to prevent accidental tampering. 

## STEP 1: Detecting EFS Encryption with FTK Imager
**IMPORTANT:** The drive's file system must be NTFS to utilize EFS encryption. EFS encryption is not compatible with FAT32 or exFAT file systems. 

![../../PNG PDF JPEG/attachment-01272026.png](../../PNG%20PDF%20JPEG/attachment-01272026.png)
![../../PNG PDF JPEG/attachment-01272026-1.png](../../PNG%20PDF%20JPEG/attachment-01272026-1.png)
![../../PNG PDF JPEG/attachment-01272026-2.png](../../PNG%20PDF%20JPEG/attachment-01272026-2.png)
![../../PNG PDF JPEG/attachment-01272026-3.png](../../PNG%20PDF%20JPEG/attachment-01272026-3.png)
![../../PNG PDF JPEG/attachment-01272026-4.png](../../PNG%20PDF%20JPEG/attachment-01272026-4.png)
A message box will indicate whether or not EFS encryption is on the attached drive.

# Using FTK Imager to Acquire Digital Artefacts and Evidence
---
## STEP 2: Creating a Forensic Disk Image with FTK Imager

![../../PNG PDF JPEG/attachment-01272026-13.png](../../PNG%20PDF%20JPEG/attachment-01272026-13.png)
![../../PNG PDF JPEG/attachment-01272026-12.png](../../PNG%20PDF%20JPEG/attachment-01272026-12.png)
![../../PNG PDF JPEG/attachment-01272026-11.png](../../PNG%20PDF%20JPEG/attachment-01272026-11.png)
![../../PNG PDF JPEG/attachment-01272026-10.png](../../PNG%20PDF%20JPEG/attachment-01272026-10.png)
Ensure you check **"Verify images after they are created"** and **"Create directory listings of all files in the image after they are created"** on the **Create Image** window.

![../../PNG PDF JPEG/attachment-01272026-9.png](../../PNG%20PDF%20JPEG/attachment-01272026-9.png)
![../../PNG PDF JPEG/attachment-01272026-8.png](../../PNG%20PDF%20JPEG/attachment-01272026-8.png)
![../../PNG PDF JPEG/attachment-01272026-7.png](../../PNG%20PDF%20JPEG/attachment-01272026-7.png)
![../../PNG PDF JPEG/attachment-01272026-6.png](../../PNG%20PDF%20JPEG/attachment-01272026-6.png)
![../../PNG PDF JPEG/attachment-01272026-5.png](../../PNG%20PDF%20JPEG/attachment-01272026-5.png)
When you check **"Verify images after they are created"**, FTK Imager will hash both the physical drive and the forensic disk image after disk imaging. It will then validate if both hashes are equal to confirm a match. 

## STEP 3: Mounting a Forensic Disk Image and Extracting Artefacts
**IMPORTANT:** During forensic analysis with FTK Imager, it is always crucial to analyse using the forensic disk image that has been created. It is also equally important to look for signs of deleted files (i.e., those with an x symbol), corrupted files (e.g., 0 file size) and obfuscation (e.g., conflicting information about a file's extension and header information). 

![../../PNG PDF JPEG/attachment-01282026.png](../../PNG%20PDF%20JPEG/attachment-01282026.png)
![../../PNG PDF JPEG/attachment-01282026-1.png](../../PNG%20PDF%20JPEG/attachment-01282026-1.png)
![../../PNG PDF JPEG/attachment-01282026-2.png](../../PNG%20PDF%20JPEG/attachment-01282026-2.png)
![../../PNG PDF JPEG/attachment-01282026-3.png](../../PNG%20PDF%20JPEG/attachment-01282026-3.png)
![../../PNG PDF JPEG/attachment-01282026-4.png](../../PNG%20PDF%20JPEG/attachment-01282026-4.png)
![../../PNG PDF JPEG/attachment-01282026-5.png](../../PNG%20PDF%20JPEG/attachment-01282026-5.png)
![../../PNG PDF JPEG/attachment-01282026-6.png](../../PNG%20PDF%20JPEG/attachment-01282026-6.png)
![../../PNG PDF JPEG/attachment-01282026-7.png](../../PNG%20PDF%20JPEG/attachment-01282026-7.png)
![../../PNG PDF JPEG/attachment-01282026-8.png](../../PNG%20PDF%20JPEG/attachment-01282026-8.png)
![../../PNG PDF JPEG/attachment-01282026-9.png](../../PNG%20PDF%20JPEG/attachment-01282026-9.png)
![../../PNG PDF JPEG/attachment-01282026-10.png](../../PNG%20PDF%20JPEG/attachment-01282026-10.png)
![../../PNG PDF JPEG/attachment-01282026-11.png](../../PNG%20PDF%20JPEG/attachment-01282026-11.png)
![../../PNG PDF JPEG/attachment-01282026-12.png](../../PNG%20PDF%20JPEG/attachment-01282026-12.png)
![../../PNG PDF JPEG/attachment-01282026-13.png](../../PNG%20PDF%20JPEG/attachment-01282026-13.png)
To recover all deleted files, right-click on the target directory or file and press **Export Files** to save artifacts.

![../../PNG PDF JPEG/attachment-01282026-14.png](../../PNG%20PDF%20JPEG/attachment-01282026-14.png)
![../../PNG PDF JPEG/attachment-01282026-15.png](../../PNG%20PDF%20JPEG/attachment-01282026-15.png)

# Case B4DM755: At the Forensics Laboratory
---
Upon receiving the artefacts and evidence from the crime scene at the Forensics Lab, it is imperative to establish their authenticity. Since the DFIR First Responders recovered only a flash drive, you then proceed with the following actions: 
- Verify and document every detail of the Chain of Custody form from the crime scene to the present. 
- Use FTK Imager to create a forensic disk image of the seized flash drive from the suspect's (William S. McClean) residence in Case B4DM755. 
- Match the cryptographic hashes of the physical drive and the acquired forensic image to guarantee the authenticity and integrity of the artefacts, making them admissible evidence in a court of law. 
- Preserve the physical evidence (i.e., flash drive) for presentation in a court of law during trial after creating a forensic disk image. 
- Perform any review and analysis on the created forensic disk image to avoid tampering with evidence. 
Document all examination operations and activities to ensure the admissibility of evidence in court. 
- During a presentation at trial, ensure that the cryptographic hashes of the physical evidence and the forensic disk image MATCH. 

# Post-Analysis of Evidence to Court Proceedings
---
 ## **Pre-search**
- Send a request to preserve the data and logs of the suspect to social media networks (subscriber's information, traffic, and content data).
- Send a request to preserve the data and logs of the suspect to ISPs (subscriber's information, traffic, and content data).
- Obtain a warrant for search, seizure, and examination of the suspect's computer data for violation of domestic and international laws.
- Perform an inspection of the suspect's social media accounts and public profiles.

 ## **Search**
- By a warrant issued by a court of law, obtain data requested from social media networks and ISPs.
- Perform search, seizure, and examination of the suspect's computer data.
## Post-search
- Perform forensic analysis of acquired digital artefacts & evidence.
## Trial
- Present forensic artefacts & evidence together with proper documentation during court proceedings.