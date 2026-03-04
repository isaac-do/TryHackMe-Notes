---
date: 2026-03-02
tags:
  - metasploit
  - exploitation
  - offensive-security
---
*Meterpreter is a Metasploit payload that supports the penetration testing process with many valuable components. Meterpreter will run on the target system and act as an agent within a command and control architecture. You will interact with the target operating system and files and use Meterpreter's specialized commands.*

*Meterpreter runs on the target system but is not installed on it. It runs in memory and does not write itself to the disk on the target.*

*Meterpreter also aims to avoid being detected by network-based IPS (Intrusion Prevention System) and IDS (Intrusion Detection System) solutions by using encrypted communication with the server where Metasploit runs (typically your attacking machine). If the target organization does not decrypt and inspect encrypted traffic (e.g. HTTPS) coming to and going out of the local network, IPS and IDS solutions will not be able to detect its activities.* 

# Meterpreter Flavors
---
Use the command `msfvenom --list payloads | grep meterpreter` to see available Meterpreter versions.
- Will list versions available on platforms Android, Apple IOS, Java, Linux, etc.

Decision on which version to choose mainly depends on:
1. Target OS
2. Components available in the target (Is Python installed? Is this a PHP website?)
3. Network connection types on the target (Do they allow raw TCP connections?)

# Post-Exploitation with Meterpreter
---
## Migrate
You can migrate Meterpreter to another process so that it can interact with it.
For example, migrating Meterpreter to Notepad.exe to start capturing keystrokes.
Example Usage:
```shell
meterpreter > migrate 716
[*] Migrating from 1304 to 716...
[*] Migration completed successfully.
meterpreter >
```
**Note:** Be careful; you may lose your user privileges if you migrate from a higher privileged (e.g. SYSTEM) user to a process started by a lower privileged user (e.g. webserver). You may not be able to gain them back.

## Hashdump
List the content of the SAM database.
Example Usage:
```shell
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
meterpreter >
```

## Search
Useful for finding files with potentially juicy information. 
Example Usage:
```shell
meterpreter > search -f flag2.txt
Found 1 result...
    c:\Windows\System32\config\flag2.txt (34 bytes)
meterpreter >
```


