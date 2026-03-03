---
date: 2026-01-21
tags:
  - dfir
  - memory
---
# Volatile Memory
---
## Memory Hierarchy
From fastest to slowest.
![attachment-01212026](../../PNG%20PDF%20JPEG/attachment-01212026.png)
- CPU registers and cache are extremely fast but limited in size.
- Disk storage is much slower but is used for long-term data retention.

**Swap** is a reserved space on the disk that the OS uses to store data from RAM when physical memory is full temporarily.

## RAM Structure
RAM is divided into two space: kernel space and user space.
- **Kernel Space:** reserved for the OS and low-level services.
- **User space:** contains processes launched by the user or applications.
  Each application gets it own space to protect it from others.

### Memory regions

| Name               | Description                                                    |
| ------------------ | -------------------------------------------------------------- |
| Stack              | Stores temp data like function arguments and return addresses. |
| Heap               | Dynamic memory allocation during runtime (objects, buffers).   |
| Executable (.text) | Stores actual code or instructions the CPU runs.               |
| Data sections      | Store global variables and other data the executable needs.    |

# Memory Dump
---
*A memory dump is a snapshot of a system's RAM at a specific point in time.*

## Types of Memory Dumps

| Name                       | Description                                                                                 |
| -------------------------- | ------------------------------------------------------------------------------------------- |
| Full Memory Dump           | Captures all RAM, including user and kernel space.                                          |
| Process Dump               | Captures the memory of a single running process.                                            |
| Pagefile and Swap Analysis | Offloaded memory content in disks. Windows: pagefile.sys, Linux swapfile or swap partition. |
# Challenges in Memory Acquisition
---

| Name                                     | Description                                                                                                                     |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Unlinked or hidden modules               | Malware may unlink itself from process lists, making it invisible to tools relying on typical OS queries.                       |
| DKOM (Direct Kernel Object Manipulation) | Alters kernel structures to hide processes , threads, or drivers from standard system tools.                                    |
| Code injection                           | Malicious code is injected into legitimate processes.                                                                           |
| Memory patching                          | Malware modifies memory content or system APIs at runtime to disrupt forensic tools or product false data.                      |
| Hooking APIs or syscalls                 | Intercepts and alters the output of critical functions to hide malicious behavior.                                              |
| Encrypted or packed payloads             | Payloads are kept encrypted or compressed in memory, only decrypted at execution time, making <u>static analysis </u>difficult. |
| Trigger-based payloads                   | Some memory-resident malware only unpacks or runs when specific conditions are met.                                             |
| Process hollowing                        | A technique where memory of a trusted process is replaced with malicious code.                                                  |
# Memory Analysis Attack Fingerprints
---
## Common Attacks Detected During Memory Analysis
### Credential Access (MITRE ATT&CK: T1003)
**T1071 - Application Layer Protocol: Command and Control**
- This filesless malware uses memory-resident payloads to fetch commands or exfiltrate data through standard protocols (HTTP, HTTPS, DNS).
- In memory, analysts can find decrypted C2 configurations, IP addresses, or beacons that are not logged anywhere else.

### In-Memory Script Execution (T1086)
**T1086 - PowerShell**
- Attackers use PowerShell or other interpreters to execute code in RAM. Memory analysis may reveal script contents, encoded commands, or runtime artifacts in the PowerShell process memory.

### Persistence Technique in Memory
**T1053.005 - Scheduled Task/Job: Scheduled Task**
- During memory analysis, we can look for processes like `schtasks.exe` and memory strings that contain task names or malicious payload paths.
**T1543.003 - Create or Modify System Access: Windows Service**
- Malicious services may run in the background under `services.exe`.
- We may find unusual service names, binaries, or configurations in memory that can relate to this technique.
**T1547.001 - Registry Run Keys / Startup Folder**
- Malware adds entries to `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` to execute on boot.
- These values can be recovered from memory or found within registry hives cached in RAM.

### Lateral Movement via Memory Artifacts
**T1021.002 - Remote Service: SMB/Windows Admin Shares (PsExec)**
- `PsExec` enables command execution on remote systems.
- Analysts may find PsExec-related services or command-line arguments in memory indicating lateral movement.
**T1021.006 - Remote Services: Windows Remote Management (WinRM)**
- `WinRM` provides PowerShell remoting.
- Look for `wsmprovhost.exe` and memory references to remote session initialization.
**T1059.001 - Command and Scripting Interpreter: PowerShell (Remote)**
- Analysts can detect base64-encoded or obfuscated commands within memory of the PowerShell process.
**T1047 - Windows Management Instrumentation (WMI)**
- WMI commands like wmic process call create may be used to spawn remote processes. 
- Associated strings or class references may be cached in memory.