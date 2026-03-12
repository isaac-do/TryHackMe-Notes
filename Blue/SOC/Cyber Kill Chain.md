---
date: 2026-03-11
tags:
  - defensive-security
  - kill-chain
  - offensive-security
---
# Introduction
---
The Cyber Kill Chain framework was established by Lockheed Martin based on military concepts. The framework defines the steps used by adversaries or malicious actors in the cyberspace.

# Reconnaissance
---
Adversaries use this phase to gather information about their target to inform their next steps.
OSINT can be used in the recon phase where adversaries can make use of public information about their target.

**Passive Recon:** This involves having no direct interaction with the target.
- Activities include WHOIS lookups, social media scraping, or reviewing breach data.
**Active Recon:** This involves direct contact with the target.
- Activities include social engineering, port scanning, banner grabbing, or probing for open services.

# Weaponization
---
In the Weaponization phase, the attacker work on turning raw information (from recon phase) into actionable attack tools through crafting **malware** and **exploits** into a **payload**.

**Key Terms**

| Term     | Description                                                                                           |
| -------- | ----------------------------------------------------------------------------------------------------- |
| Malware  | A program or software that is designed to damage, disrupt, or gain unauthorized access to a computer. |
| Exploits | Programs or code that take advantage of the vulnerability or flaw in the application or system.       |
| Payload  | A malicious code that the attacker runs on the system.                                                |

# Delivery
---
In the Delivery phase, the attacker choose the method for transmitting the payload or the malware onto the target environment.

Some examples include:
- Phishing emails
- USB drops
- Watering hole attacks

# Exploitation
---
The Exploitation phase is the moment the attacker's code executes on the target, taking advantage of a known vulnerability.

After gaining access to the system, the malicious actor could exploit software, system, or server-based vulnerabilities to escalte the privileges or move laterally through the network.

# Installation
---
After the attacker have exploited the target's vulnerabilities to gain access to a network, they begin the Installation phase. 
The attacker attempts to install malware and other cyberweapons onto the target network to take control of its systems and exfiltrate valuable data.


# Command & Control
---
In the Command & Control phase, the attacker communicate with the malware they've installed onto a target's network to instruct cyberweapons or tools to carry out their objectives.

**C&C** or **C2 Beaconing** is a type of malicious communication between a C&C server and malware on the infected host.

The most common C2 channels used by adversaries include:
- HTTP/80 and HTTPS/443, since this will blend malicious traffic with legitimate traffic
- DNS (DNS Tunneling), where the infected machine makes constant DNS requests to the DNS server that belongs to an attacker

# Actions on Objectives (Exfiltration)
---
After going through the six phaes of the attack, the attacker can begin to carry out their cyberattack objectives.