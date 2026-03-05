---
date: 2026-03-04
tags:
  - snort
  - network
  - defensive-security
  - linux
---
# Snort Modes
---
Snort is a widely used open-source IDS solutions that uses signature-based and anomaly-based detections to identify known threats. 

| Mode                | Description                                                                                                                                                                                                                                                                                                                           |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Packet sniffer mode | This mode reads and displays network packets without performing any analysis on them. The packet sniffer mode of Snort does not directly relate to IDS capabilities, but it can be helpful in network monitoring and troubleshooting. This mode allows you to display the network traffic on the console or even output it in a file. |
| Packet logging mode | The packet logging mode of Snort allows you to log the network traffic as a PCAP (standard packet capture format) file. This includes all the network traffic and any detections from it.                                                                                                                                             |
| NIDS mode           | Snort’s NIDS mode is the primary mode that monitors network traffic in real-time and applies its rule files to identify any match to the known attack patterns stored as signatures. If there is a match, it generates an alert. This mode provides the main functionality of an IDS solution.                                        |

# Snort Usage
---
The default directory for snort is `/etc/snort`

## Rule Format
![](../../PNG%20PDF%20JPEG/attachment-03042026-3.png)
- **Action:** This specifies which action to take when the rule triggers.
- **Protocol:** This refers to the protocol that matches this rule. 
- **Source IP:** This determines the IP originating from the traffic. 
- **Source port:** This determines the port from which the traffic originates.
- **Destination IP:** This specifies the destination IP to which the matching traffic comes; it generates the alert. In this case, we used "$HOME_NET". This is a variable, and we defined its value as our whole network’s range in Snort’s configuration file.
- **Destination port:** This specifies the port the traffic would reach.
- **Rule metadata:** Every rule has some metadata. That is defined at the end of the rule in parentheses. The following are its components:
	- **Message (msg):** This describes the message displayed when the subject rule triggers. The message should indicate the type of activity detected.
	- **Signature ID (sid):** Every rule has a unique identifier that differentiates it from others. This identifier is called the signature ID (sid).
	- **Rule revision (rev):** This sets the rule's revision number. Every time the rule is modified, its revision number is incremented, which helps in tracking changes to any rule.

Execute the following command to tell Snort to listen on the loopback interface:
```shell
ubuntu@tryhackme:~$ sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf
```

Execute the following command to open a PCAP file using Snort:
```shell
ubuntu@tryhackme:~$ sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf
```
