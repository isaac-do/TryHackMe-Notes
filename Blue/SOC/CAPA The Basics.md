---
date: 2026-03-05
tags:
  - capa
  - defensive-security
  - mitre
---
# CAPA Overview
---
**CAPA (Common Analysis Platform for Artifacts)** is a tool developed by the FireEye Mandiant team. It is designed to identify the capabilities present in executable files like Portable Executables (PE), ELF binaries, .NET modules, shellcode, and even sandbox reports. It does so by analyzing the file and applying a set of rules that describe common behaviours, allowing it to determine what the program is capable of doing, such as network communication, file manipulation, process injection, and many more.

| Option            | Description                            |
| ----------------- | -------------------------------------- |
| -h or --help      | Show this help message and exit.       |
| -v or --verbose   | Enable verbose result document.        |
| -vv or --vverbose | Enable a very verbose result document. |

Example Usage:
```powershell
PS C:\Users\Administrator\Desktop\capa> capa .\cryptbot.bin
┌─────────────┬─────────────────────────────────────────────────────┐
│ md5         │ 3b9d26d2e7433749f2c32edb13a2b0a2                    │
│ sha1        │ 969437df8f4ad08542ce8fc9831fc49a7765b7c5            │
│ sha256      │ ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c    │
│ analysis    │ static                                              │
│ os          │ windows                                             │
│ format      │ pe                                                  │
│ arch        │ i386                                                │
│ path        │ /home/strategos/Room-CAPA/cryptbot.bin              │
└─────────────┴─────────────────────────────────────────────────────┘
┌──────────────────────┬────────────────────────────────────────────────┐
│ ATT&CK Tactic        │ ATT&CK Technique                               │
├───────────────────────────────────────────────────────────────────────┤
│ DEFENSE EVASION      │ Obfuscated Files or Information [T1027]        │
│                      │ Obfuscated Files or Information::Indicator Removal from Tools [T1027.005] │
│                      │ Virtualization/Sandbox Evasion::System Checks [T1497.001]            │
│ DISCOVERY            │ File and Directory Discovery [T1083]           │
│ EXECUTION            │ Command and Scripting Interpreter::PowerShell [T1059.001]            │
│                      │ Shared Modules [T1129]                         │
│ IMPACT               │ Resource Hijacking [T1496]                     │
│ PERSISTENCE          │ Scheduled Task/Job::At [T1053.002]             │
│                      │ Scheduled Task/Job::Scheduled Task [T1053.005] │
└──────────────────────┴────────────────────────────────────────────────┘
┌─────────────────────────────┬─────────────────────────────────────────┐
│ MAEC Category               │ MAEC Value                              │
├─────────────────────────────┼─────────────────────────────────────────┤
│ malware-category            │ launcher                                │
└─────────────────────────────┴─────────────────────────────────────────┘
┌──────────────────────────┬────────────────────────────────────────────┐
│ MBC Objective            │ MBC Behavior                               │
├──────────────────────────┼────────────────────────────────────────────┤
│ ANTI-BEHAVIORAL ANALYSIS │ Virtual Machine Detection [B0009]          │
│ ANTI-STATIC ANALYSIS     │ Executable Code Obfuscation::Argument Obfuscation [B0032.020]    │
│                          │ Executable Code Obfuscation::Stack Strings [B0032.017]                │
│ COMMUNICATION            │ HTTP Communication [C0002]                 │
│                          │ HTTP Communication::Read Header [C0002.014]│
│ DATA                     │ Check String [C0019]                       │
│                          │ Encode Data::Base64 [C0026.001]            │
│                          │ Encode Data::XOR [C0026.002]               │
│ DEFENSE EVASION          │ Obfuscated Files or Information::Encoding-Standard Algorithm [E1027.m02] │
│ DISCOVERY                │ File and Directory Discovery [E1083]       │
│ EXECUTION                │ Command and Scripting Interpreter [E1059]  │
│ FILE SYSTEM              │ Create Directory [C0046]                   │
│                          │ Delete File [C0047]                        │
│                          │ Read File [C0051]                          │
│                          │ Writes File [C0052]                        │
│ MEMORY                   │ Allocate Memory [C0007]                    │
│ PROCESS                  │ Create Process [C0017]                     │
└──────────────────────────┴────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────┬────────────────┐
│ Capability                                           │ Namespace      │
├──────────────────────────────────────────────────────┼────────────────┤
│ reference anti-VM strings                            │ anti-analysis/anti-vm/vm-detection           │
│ reference anti-VM strings targeting VMWare           │ anti-analysis/anti-vm/vm-detection           │
│ reference anti-VM strings targeting VirtualBox       │ anti-analysis/anti-vm/vm-detection           │
│ contain obfuscated stackstrings (2 matches)          │ anti-analysis/obfuscation/string/stackstring │
│ reference HTTP User-Agent string                     │ communication/http                           │
│ check HTTP status code                               │ communication/http/client                    │
│ reference Base64 string                              │ data-manipulation/encoding/base64            │
│ encode data using XOR                                │ data-manipulation/encoding/xor               │
│ contain a thread local storage (.tls) section        │ executable/pe/section/tls                    │
│ get common file path                                 │ host-interaction/file-system                 │
│ create directory                                     │ host-interaction/file-system/create          │
│ delete file                                          │ host-interaction/file-system/delete          │
│ read file on Windows (4 matches)                     │ host-interaction/file-system/read            │
│ write file on Windows (5 matches)                    │ host-interaction/file-system/write           │
│ get thread local storage value                       │ host-interaction/process                     │
│ create process on Windows                            │ host-interaction/process/create              │
│ allocate or change RWX memory                        │ host-interaction/process/inject              │
│ reference cryptocurrency strings                     │ impact/cryptocurrency                        │
│ link function at runtime on Windows (5 matches)      │ linking/runtime-linking                      │
│ parse PE header (4 matches)                          │ load-code/pe                                 │
│ resolve function by parsing PE exports (186 matches) │ load-code/pe                                 │
│ run PowerShell expression                            │ load-code/powershell/                        │
│ schedule task via at                                 │ persistence/scheduled-tasks                  │
│ schedule task via schtasks                           │ persistence/scheduled-tasks                  │
└──────────────────────────────────────────────────────┴────────────────┘
```

# Dissecting CAPA Results: General Information, MITRE and MAEC
---
The first block contains basic information about the file.
```powershell
┌─────────────┬─────────────────────────────────────────────────────┐
│ md5         │ 3b9d26d2e7433749f2c32edb13a2b0a2                    │
│ sha1        │ 969437df8f4ad08542ce8fc9831fc49a7765b7c5            │
│ sha256      │ ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c    │
│ analysis    │ static                                              │
│ os          │ windows                                             │
│ format      │ pe                                                  │
│ arch        │ i386                                                │
│ path        │ /home/strategos/Room-CAPA/cryptbot.bin              │
└─────────────┴─────────────────────────────────────────────────────┘
```

## MITRE ATT&CK
CAPA uses this format for the output. Note that some results may or may not contain the Technique and Sub-technique Identifier. 

| Format                                                                                                 | Sample                                                                                   |
| ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| ATT&CK Tactic::ATT&CK Technique::Technique Identifier                                                  | Defense Evasion::Obfuscated Files or Information::T1027                                  |
| ATT&CK Tactic::ATT&CK Technique::ATT&CK Sub-Technique::Technique Identifier[.]Sub-technique Identifier | Defense Evasion::Obfuscated Files or Information::Indicator Removal from Tools T1027.005 |

```powershell
┌──────────────────────┬────────────────────────────────────────────────┐
│ ATT&CK Tactic        │ ATT&CK Technique                               │
├───────────────────────────────────────────────────────────────────────┤
│ DEFENSE EVASION      │ Obfuscated Files or Information [T1027]        │
│                      │ Obfuscated Files or Information::Indicator Removal from Tools [T1027.005] │
│                      │ Virtualization/Sandbox Evasion::System Checks [T1497.001]            │
│ DISCOVERY            │ File and Directory Discovery [T1083]           │
│ EXECUTION            │ Command and Scripting Interpreter::PowerShell [T1059.001]            │
│                      │ Shared Modules [T1129]                         │
│ IMPACT               │ Resource Hijacking [T1496]                     │
│ PERSISTENCE          │ Scheduled Task/Job::At [T1053.002]             │
│                      │ Scheduled Task/Job::Scheduled Task [T1053.005] │
└──────────────────────┴────────────────────────────────────────────────┘
```


## MAEC
**Malware Attribute Enumeration and Characterization** is a specialized language designed to encode and communicate complex details concerning malware.

Commonly used MAEC values by CAPA:

| MAEC Value | Description                                                                                              | Examples                                                                                                                                                    |
| ---------- | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Launcher   | Exhibits behaviours that trigger specific actions similar to malware behaviour.                          | - Dropping additional payloads<br>- Activating persistence mechanisms<br>- Connecting to command-and-control (C2) servers<br>- Executing specific functions |
| Downloader | Exhibits behaviours wherein it downloads and executes other files, usually seen on more complex malware. | - Fetching additional payloads or resources from the internet<br>- pulling in updates<br>- executing secondary stages<br>- retrieving configuration files   |

```powershell
┌─────────────────────────────┬─────────────────────────────────────────┐
│ MAEC Category               │ MAEC Value                              │
├─────────────────────────────┼─────────────────────────────────────────┤
│ malware-category            │ launcher                                │
└─────────────────────────────┴─────────────────────────────────────────┘
```

# Dissecting CAPA Results: Malware Behavior Catalogue (MBC)
---
MBC is designed to support various aspects of malware analysis, such as labelling, similarity analysis, and standardized reporting. Essentially, it serves as a catalogue of malware objectives and behaviours.

| Format                                    | Sample                                                                                |
| ----------------------------------------- | ------------------------------------------------------------------------------------- |
| `OBJECTIVE::Behavior::Method[Identifier]` | `ANTI-STATIC ANALYSIS::Executable Code Obfuscation::Argument Obfuscation [B0032.020]` |
| `OBJECTIVE::Behavior::[Identifier]`       | `COMMUNICATION::HTTP Communication:: [C0002]`                                         |

## Obective
The Objective are based on ATT&CK tactics in the context of malware behaviour, though not all are included.
MBC has additional Anti-Behavioral and Anti-Static Analysis.

## Micro-Objective
Micro-objectives are associated with micro-behaviors, which refer to an action or actions exhibited by potentially malicious software that isn't necessarily malicious and may serve various objectives. However, it's important to note that these behaviours are typically abused.

## MBC Behaviors
The column MBC Behaviors contains behaviours and Micro-behaviors with or without its methods and identifiers. 

See [Link](https://github.com/MBCProject/mbc-markdown/blob/main/mbc_summary.md) for a listing of all MBC content.

## Micro-Behavior
The term "low-level behaviors" in malware analysis refers to actions exhibited by malware that aren't necessarily malicious and may serve various objectives. These behaviors are often documented as "micro-behaviors" in the Malware Behavior Characteristics (MBC) analysis. It's important to note that just because a behavior is categorized as low-level does not mean it is harmless, as it may still be part of a larger malicious scheme.

## Methods
Methods are tied to behaviors; therefore, to fully see all methods, please refer to each specific behavior/micro behavior of interest.

Example:
```powershell
┌─────────────────────────────┬──────────────────────────────────┐
│ MBC Objective               │ MBC Behavior                     │
├─────────────────────────────┼──────────────────────────────────┤
| DATA                        │ Encode Data::Base64 [C0026.001]  │
└─────────────────────────────┴──────────────────────────────────┘
```

| Label         | Value       | Explanation                                                                                                   |
| ------------- | ----------- | ------------------------------------------------------------------------------------------------------------- |
| MBC Objective | DATA        | exhibiting behaviors such as, but not limited to, checking strings, compressing, decoding, and encoding data. |
| MBC Behavior  | Encode Data | Malware has the capability to encode data using base64 and XOR.                                               |
| Method        | Base64      | Malware may encode data using Base64.                                                                         |
| Identifier    | C0026.001   | identifier relays information about a behavior. This also serves as a tag.                                    |

# Dissecting CAPA Results: Namespaces
---
CAPA uses namespaces to group items with the same purpose.  

| Format                                                      | Sample                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------------- |
| `Capability(Rule Name)::TLN(Top-Level Namespace)/Namespace` | reference anti-VM strings::Anti-Analysis/anti-vm/vm-detection |

```powershell
┌─────────────────────────────────┬─────────────────────────────────────┐
│ Capability                      │ Namespace                           │
├─────────────────────────────────┼─────────────────────────────────────┤
│ reference anti-VM strings       │ anti-analysis/anti-vm/vm-detection  │
└─────────────────────────────────┴─────────────────────────────────────┘
```

See [link](https://github.com/MBCProject/capa-rules-1?tab=readme-ov-file#namespace-organization) for more information in other TLNs.

# Dissecting CAPA Results: Capability
---
Capability Example:

| Capability                | TLN           | Namespaces           | Rule YAML File                |
| ------------------------- | ------------- | -------------------- | ----------------------------- |
| reference anti-VM strings | Anti-Analysis | anti-vm/vm-detection | reference-anti-vm-strings.yml |
| check HTTP status code    | Communication | http                 | check-http-status-code.yml    |
**Note:** The item under Capability has the same name as the YML files under the Rules, with the addition of a dash (-) character between spaces. Simply because Capability is the name of the rule. 

```powershell
┌─────────────────────────────────┬─────────────────────────────────────┐
│ Capability                      │ Namespace                           │
├─────────────────────────────────┼─────────────────────────────────────┤
│ reference anti-VM strings       │ anti-analysis/anti-vm/vm-detection  │
└─────────────────────────────────┴─────────────────────────────────────┘
```

| Label                   | Value                       |
| ----------------------- | --------------------------- |
| Capability              | reference base64 string     |
| Top-Level Namespace     | data-manipulation           |
| Namespace               | encoding/base64             |
| Rule YAML File Matched? | reference-base64-string.yml |

# CAPA Web Explorer
---
See the online web version of CAPA at this [link](https://mandiant.github.io/capa/explorer/#/).