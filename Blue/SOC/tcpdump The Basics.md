---
date: 2026-02-06
tags:
  - tcpdump
  - network
---
# Basic Packet Capture
---
## Specify Network Interface
Use the `-i` argument to listen to a network interface.
- Use `-i INTERFACE` to listen to a specific network interface.
- Use `-i any` to listen to all available network interfaces.

Use `ip a s` (a=address, s=show) command to list the available network interfaces.

## Save the Captured Packets
Save packets using the argument `-w FILE` to save the packets to a file. The file extension will be `.pcap`.

## Read Captured Packets From a File
Use the argument `-r FILE` to read packets from a file. 

## Limit the Number of Captured Packets
Use the argument `-c COUNT` to specify the number of packets to capture.
Without specifying a count, the packet capture will continue until user interrupts (CTRL+C).

## Don't Resolve IP Addresses and Port Numbers
Tcpdump will resolve IP addresses and print the friendly domain name when possible.
- Use the argument -n to avoid making DNS lookups.
- Use the argument -nn to stop both DNS and port number lookups.

# Produce More Details on Packets
Use the argument `-v` to produce a slightly more verbose output, `-vv` even more verbose output, and `-vvv` to produce the mose verbose output.

## Summary

| Command                  | Explanation                                                    |
| ------------------------ | -------------------------------------------------------------- |
| tcpdump -i `<INTERFACE>` | Captures packets on a specific network interface.              |
| tcpdump -w `<FILE>`      | Writes captured packets to a file.                             |
| tcpdump -r `<FILE>`      | Reads captured packets from a file.                            |
| tcpdump -c `<COUNT>`     | Captures a specific number of packets.                         |
| tcpdump -n               | Don't resolve IP addresses.                                    |
| tcpdump -nn              | Don't resolve IP addresses and don't resolve protocol numbers. |
| tcpdump -v               | Verbose display; verbosity increases using -vv and -vvv        |

# Filtering Expressions
---
It is important to note that capturing packets requires you to be logged-in as root or to use sudo.

| Command                                      | Explanation                                                      |
| -------------------------------------------- | ---------------------------------------------------------------- |
| tcpdump host IP or tcpdump host `<HOSTNAME>` | Filters packets by IP address or hostname.                       |
| tcpdump src `<host IP>`                      | Filters packets by a specific source host.                       |
| tcpdump dst `<host IP>`                      | Filters packets by a specific destination host.                  |
| tcpdump port `<PORT_NUMBER>`                 | Filters packets by port number.                                  |
| tcpdump src port `<PORT_NUMBER>`             | Filters packets by the specified source port number.             |
| tcpdump dst port `<PORT_NUMBER>`             | Filters packets by the specified destination port number.        |
| tcpdump `<PROTOCOL>`                         | Filters packets by protocol; examples include ip, ip6, and icmp. |
# Advanced Filtering
---
## Binary Operations

| Input 1 | Input 2 | Input 1 & Input 2 |
| ------- | ------- | ----------------- |
| 0       | 0       | 0                 |
| 0       | 1       | 0                 |
| 1       | 0       | 0                 |
| 1       | 1       | 1                 |
|         |         |                   |
The `&` (and) operator returns 0 unless both inputs are 1.

| Input 1 | Input 2 | Input 1 \| Input 2 |
| ------- | ------- | ------------------ |
| 0       | 0       | 0                  |
| 0       | 1       | 1                  |
| 1       | 0       | 1                  |
| 1       | 1       | 1                  |
The `|` (or) operator returns 1 unless both inputs are 0.

| Input 1 | ! Input 1 |
| ------- | --------- |
| 0       | 1         |
| 1       | 0         |
The `!` (not) operator takes one bit and inverts it.

## Header Bytes
Use `tcp[tcpflags]` to refer to TCP flags field.
- tcp-syn
- tcp-ack
- tcp-fin
- tcp-rst
- tcp-push
Example:
```bash
tcpdump "tcp[tcpflags] == tcp-syn"
tcpdump "tcp[tcpflags] & tcp-syn != 0"
tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"
```

# Displaying Packets
---

| Command     | Explanation                                                 |
| ----------- | ----------------------------------------------------------- |
| tcpdump -q  | Quick output; print brief packet information.               |
| tcpdump -e  | Print the link-level header. Includes MAC addresses.        |
| tcpdump -A  | Show packet data in ASCII.                                  |
| tcpdump -XX | Show packet data in hexadecimal format, referred to as hex. |
| tcpdump -X  | Show packet headers and data in hex and ASCII.              |
