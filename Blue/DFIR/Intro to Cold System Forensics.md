---
date: 2026-01-21
tags:
  - dfir
  - forensics
  - imaging
  - disk-imaging
  - chain-of-custody
---
**Cold boot attack:** Attackers take advantage of the data that persists in RAM for a short period depending on the RAM's temperature. If the RAM was cooler, the longer the data can be retained.
Attackers could rapidly reboot a compromised machine to access sensitive information before it is completely erased.

**Cold System Forensics** focuses on investigating dormant or <u>powered-off</u> machines, aiming to preserve the system's state and any potential evidence.

# Order of Volatility
---
Most Volatile to least volatile:
- CPU registers and cache
- Routing Table, ARP Cache, Process Table, Kernel Stats, RAM
- Temp File Systems
- Hard Disk
- Remote Logging and Monitoring Data
- Physical Configuration and Network Topology
- Archival Media

See [Memory Analysis Introduction](Memory%20Analysis%20Introduction.md) for the list of fastest memory to slowest memory.

# Methods for Acquiring Data From Cold Systems
---
## Disk Imaging
This is the processing of creating a bit-by-bit copy of a disk.
**Write blocking** prevents data alteration during acquisition and ensures integrity of the evidence.

See [Forensic Imaging](Forensic%20Imaging.md) for more details about disk imaging.

## Physical Acquisition
This involves removing and imaging hard drives from the target system.
This method requires careful handling to avoid data corruption or damage.
Use this method when dealing with damaged devices or encrypted storage.

Some methods include:
- **Chip-off forensics:** A delicate process that involves removing the storage chip from the device and reading it with a specialized equipment.
- **JTAG forensics:** uses Joint Test Action Group (JTAG) interface to access data from embedded systems. Helpful for mobile and IoT devices.
## Secure Storage
- Strong encryption
- Access control
- Environmental control
- Regular audits

## Ensuring Integrity and Chain of Custody
***Chain of Custody** is the documentation of the responsible personnel in charge of evidence, its transfer from the point of collection to its presentation in court or required body after investigation.*

Follow these guidelines to maintain integrity and chain of custody:
- Document every step: no matter how small an action is during forensic analysis
- Secure transport
- Hashing
- Write blocking

# Forensic Tools and Techniques
---
## Disk Imaging Tools
- dd and dc3dd
- Guymager
- FTK Imager
## Disk Image Analysis Tools
- The Sleuth Kit (TSK)
- Autopsy
- EnCase Forensic
- FTK (Forensic Toolkit)
- Magnet AXIOM

## Basic Workflow for Disk Image Analysis
1. Load the Disk Image
2. Run Initial Processing
3. Conduct Artifiact Analysis
4. Recover Deleted Files
5. Bookmark and Report

## Techniques for Analyzing Disk Images
### Recovering Deleted Data
**File carving** techniques scan disk images for known file headers and footers, reconstructing files from raw data.

# Extra Resources
- See [Forensic Imaging](Forensic%20Imaging.md) for details about disk imaging and using dc3dd tool.