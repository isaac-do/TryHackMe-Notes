---
date: 2026-03-05
tags:
  - remnux
  - defensive-security
  - wireshark
  - oledump
  - inetsim
  - linux
---
# Introduction
---
The REMnux VM is a specialised Linux distro. It already includes tools like Volatility, YARA, Wireshark, oledump, and INetSim. It also provides a sandbox-like environment for dissecting potentially malicious software without risking your primary system. It's your lab set up and ready to go without the hassle of manual installations.

# File Analysis
---
`Oledump.py` is a Python tool that analyzes **OLE2** files, commonly called Structured Storage or Compound File Binary Format. 

OLE stands for Object Linking and Embedding, a proprietary technology developed by Microsoft. OLE2 files are typically used to store multiple data types, such as documents, spreadsheets, and presentations, within a single file.

Example Usage:
```shell
ubuntu@MACHINE_IP:~/Desktop/tasks/agenttesla$ oledump.py agenttesla.xlsm 
A: xl/vbaProject.bin
 A1:       468 'PROJECT'
 A2:        62 'PROJECTwm'
 A3: m     169 'VBA/Sheet1'
 A4: M     688 'VBA/ThisWorkbook'
 A5:         7 'VBA/_VBA_PROJECT'
 A6:       209 'VBA/dir'
```
- The A (index) +Numbers are called **data streams**. 
- The M means there is a Macro.

Use the `-s [NUM]` (short for `-select`) option to select a data stream.
Example:
```shell
ubuntu@MACHINE_IP:~/Desktop/tasks/agenttesla$ oledump.py agenttesla.xlsm -s 4
```

Use the `--vbadecompress` option to automatically decompress any compressed VBA macros it finds into a more readable format, making it easier to analyze the contents of the macros.
Example:
```shell
ubuntu@MACHINE_IP:~/Desktop/tasks/agenttesla$ oledump.py agenttesla.xlsm -s 4 --vbadecompress
```

# Fake Network to Aid Analysis
---
## INetSim
INetSim is a tool that can be uysed to create a simulated network.

Simply edit the `/etc/inetsim/inetsim.conf` and change `#dns_default_ip 0.0.0.0`  to the IP of the machine running the simulation.

Start the tool using `sudo inetsim`.
```shell
ubuntu@MACHINE_IP:~$ sudo inetsim
INetSim 1.3.2 (2020-05-19) by Matthias Eckert & Thomas Hungenberg
Using log directory:      /var/log/inetsim/
Using data directory:     /var/lib/inetsim/
Using report directory:   /var/log/inetsim/report/
Using configuration file: /etc/inetsim/inetsim.conf
Parsing configuration file.
Warning: Unknown option '/var/log/inetsim/report/report.104162.txt#start_service' in configuration file '/etc/inetsim/inetsim.conf' line 43
Configuration file parsed successfully.
=== INetSim main process started (PID 4859) ===
Session ID:     4859
Listening on:   MACHINE_IP
Real Date/Time: 2024-09-22 17:38:22
Fake Date/Time: 2024-09-22 17:38:22 (Delta: 0 seconds)
 Forking services...
  * dns_53_tcp_udp - started (PID 4863)
  * http_80_tcp - failed!
  * https_443_tcp - started (PID 4865)
  * ftps_990_tcp - started (PID 4871)
  * pop3_110_tcp - started (PID 4868)
  * smtp_25_tcp - started (PID 4866)
  * ftp_21_tcp - started (PID 4870)
  * pop3s_995_tcp - started (PID 4869)
  * smtps_465_tcp - started (PID 4867)
 done.
Simulation running.
```

On a second machine, navigate to `https://<SIM_MACHINE_IP>` to verify connection.

A common malware behavior is downloading files or scripts.
Example:
```shell
sudo wget https://SIM_MACHINE_IP/second_payload.zip --no-check-certificate
```

What we did here is mimic a malware's behaviour, wherein it will try to reach out to a server or URL and then download a secondary file that may contain another malware.

## Connection Report
---
If you stop INetSim on the Sim machine, by default, it will create a report on its connections usually saved to `/var/log/inetsim/report/`.

Example:
```shell
ubuntu@MACHINE_IP:~$ sudo cat /var/log/inetsim/report/report.2594.txt
=== Report for session '2594' ===

Real start date            : 2024-09-22 21:04:42
Simulated start date       : 2024-09-22 21:04:42
Time difference on startup : none

2024-09-22 21:04:53  First simulated date in log file
2024-09-22 21:04:53  HTTPS connection, method: GET, URL: https://MACHINE_IP/, file name: /var/lib/inetsim/http/fakefiles/sample.html
2024-09-22 21:16:07  HTTPS connection, method: GET, URL: https://MACHINE_IP/test.exe, file name: /var/lib/inetsim/http/fakefiles/sample_gui.exe
2024-09-22 21:18:37  HTTPS connection, method: GET, URL: https://MACHINE_IP/second_payload.ps1, file name: /var/lib/inetsim/http/fakefiles/sample.html
2024-09-22 21:18:49  HTTPS connection, method: GET, URL: https://MACHINE_IP/second_payload.zip, file name: /var/lib/inetsim/http/fakefiles/sample.html
2024-09-22 21:18:49  Last simulated date in log file
===
```
