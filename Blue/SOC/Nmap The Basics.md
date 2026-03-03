---
date: 2026-02-06
tags:
  - nmap
  - network
---
# Host Discovery: Who Is Online
---
Ways to specify Nmap targets:
- IP range using `-`: 192.168.1.1-10
- IP subnet using `/`: 192.168.0.1/24
- Hostname: `example.thm`

Discover *live hosts* with `-sn` option.
Example:
```bash
nmap -sn 192.168.0.1/24
```

List the targets to scan without actually scanning them with `-sL` option.
Example:
```bash
nmap -sL 192.168.0.1/24
```

# Port Scanning: Who Is Listening
---
Perform a **Connect** **Scan** using the `-sT` option to check for open TCP ports and allow a TCP connection to be established.

Perform a SYN only scan (Stealth) using the `-sS` arguments to show fewer logs since TCP three-way handshake never completes and the TCP connection is never established.

Scan UDP ports with `-sU` arguments.

## Limiting the Target Ports
Use the `-F` argument for Fast mode which scans the 100 most common ports (instead of default 1000).

Use `-p[range]` to specify a range of ports to scan. For example, `-p10-1024` scans port 10 to port 1024 and `-p-25` scans all ports between 1 and 25. The `-p-` scans all ports.

# Version Detection: Extract More Information
---

| Option | Explanation                                           |
| ------ | ----------------------------------------------------- |
| -O     | OS detection.                                         |
| -sV    | Service and version detection.                        |
| -A     | OS detection, version detection, and other additions. |
| -Pn    | Scan hosts that appear to be down.                    |

# Timing
---

| Timing          | Total Duration |
| --------------- | -------------- |
| T0 (paranoid)   | 9.8 hours      |
| T1 (sneaky)     | 27.53 minutes  |
| T2 (polite)     | 40.56 seconds  |
| T3 (normal)     | 0.15 seconds   |
| T4 (aggressive) | 0.13 seconds   |
Use either `-T0` or or `-T 0` or `-T paranoid` to set the nmap speed.

Use `--min-parallenlism <numprobes>` and `--max-parallelism <numprobes>` to control the number of parallel service probes.

Use `--min-rate <number>` and `--max-rate <number>` to control the minimum and maximum packet rate per second.

The `--host-timeout` is the max amount of time to wait for a target host.

# Output
---
Use `-v` option for a verbose output.
- Can increase verbosity using either `-vv` or `-v2` or `-v4` options.
Use `-d` option for debugging output.
- The max debugging level is `-d9`.

## Saving Scan Reports

| Option           | Explanation                                      |
| ---------------- | ------------------------------------------------ |
| -oN `<filename>` | Saves to normal output.                          |
| -oX `<filename>` | Saves to XML output.                             |
| -oG `<filename>` | `grep`-able output (useful for `grep` or `awk`). |
| -oA `<filename>` | Output in all major formats.                     |
# Conclusion
---
It is *best* to run Nmap with `sudo` privileges to take advantage of full features. Running Nmap with local user privileges will get minimal features and defaults to Connect Scan.