---
date: 2026-02-05
tags:
  - wireshark
  - network
---
# Tool Overview
---
*An open-source, cross-platform network packet analyser tool capable of sniffing and investigating live traffic and inspecting packet captures (PCAP). It is commonly used as one of the best packet analysis tools.*

![../../PNG PDF JPEG/attachment-02052026-1.png](../../PNG%20PDF%20JPEG/attachment-02052026-1.png)

## Loading PCAP Files
![../../PNG PDF JPEG/attachment-02052026-2.png](../../PNG%20PDF%20JPEG/attachment-02052026-2.png)
**Packet Details Panel:** detailed protocol breakdown of the selected packet.
**Packet Bytes Pane:** Hex and decoded ASCII representation of the selected packet.

## Coloring Packets
Wireshark has two types of packet coloring methods: *temporary* and *permanent*.
- Temporary rules are only available during a program session.
- Permanent rules are saved and available for the next program session.

## Viewing File Details
![../../PNG PDF JPEG/attachment-02052026.gif](../../PNG%20PDF%20JPEG/attachment-02052026.gif)

# Packet Navigation
---
## Expert Info
Wireshark also detects specific states of protocols to help analysts easily spot possible anomalies and problems. Note that these are only suggestions, and there is always a chance of having false positives/negatives. Expert info can provide a group of categories in three different severities. Details are shown in the table below.

| Severity | Color  | Info                                                     |
| -------- | ------ | -------------------------------------------------------- |
| Chat     | Blue   | Information on usual workflow.                           |
| Note     | Cyan   | Notable events like application error codes.             |
| Warn     | Yellow | Warnings like unusual error codes or problem statements. |
| Error    | Red    | Problems like malformed packets.                         |
