---
date: 2026-03-11
tags:
  - defensive-security
  - mitre
  - attck
---
# ATT&CK Framework
---
**Tactic:** An adversary's goal or objective. The "why" of an attack.
**Technique:** How an adversary achieves their goal or objective.
**Procedure:** The implementation or how the technique is executed.

![](../../PNG%20PDF%20JPEG/attachment-03112026.png)

# ATT&CK in Operation
---
ATT&CK provides a standard and consistent language for describing adversary behavior. It also provides a mapping of threat activity to TTPs allowing defenders to translate intelligence into real detection logic, queries, and playbooks.

Example mapping
![](../../PNG%20PDF%20JPEG/attachment-03112026-1.png)
> The group Mustang Panda prefers phishing techniques for initial access, persists via scheduled tasks, obfuscates files to evade defenses, and uses an ingress tool transfer for command and control.

**How different teams use ATT&CK**

| Who                            | Goal                                                                        | How They Use ATT&CK                                                                                          |
| ------------------------------ | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| Cyber Threat Intel (CTI) Teams | Collect and analyze threat information to improve an org's security posture | Map observed threat actor behavior to ATT&CK TTPs to create profiles that are actionable across the industry |
| SOC Analysts                   | Investigate and triage security alerts                                      | Link activity to tactics and techniques to provide detailed context for alerts and prioritize incidents      |
| Detection Engineers            | Design and improve detection systems                                        | Map SIEM/EDR and other rules to ATT&CK to ensure better detection efforts                                    |
| Incident Responders            | Respond to and investigate security incidents                               | Map incident timelines to MITRE tactics and techniques to better visualize the attack.                       |
| Red & Purple Team              | Emulate adversary behavior to test and improve defenses                     | Build emulation plans and exercises aligned with techniques and known group operations                       |

# Cyber Analytics Repository (CAR)
---
The Cyber Analytics Repository (CAR) is a colelction of ready-made detection analytics built around ATT&CK.
Each analytic describes how to detect an adversary's behavior.

# MITRE D3FEND Framework
---
**D3FEND:** Detection, Denial, and Disruption Framework Empowering Network Defense

This is a structured framework that maps out defensive techniques and establishes a common langauge for describing how security controls work.

D3FEND Matrix
- Model
- Harden
- Detect
- Isolate
- Deceive
- Evict
- Restore

Example:
The D3-CRO (Credential Rotation) technique emphasizes rotating passwords regularly to prevent attackers from reusing stolen credentials.
The D3FEND explains how this defense works, what to consider when implementing it, and how it relates to specific digital artifacts and ATT&CK techniques.
![](../../PNG%20PDF%20JPEG/attachment-03112026-2.png)

# Other MITRE Projects
---
The **MITRE Adversary Emulation Library** contains several emulations that mimic real-world attacks by known threat groups. The emulation plans are a step-by-step guide on how to mimic the speicifc threat group.

**Caldera** is an automated advesary emulation tool designed to help security teams test and enhance their defenses.
It provides the ability to imulate real-world attacker behavior utilizing the ATT&CK framework.

**AADAPT (Adversarial Actions in Digital Asset Payment Technologies)** covers adversary tactics and techniques related to digital asset management systems.

**ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems)** is a knowledge base and framework focusing on threats targeting artifact intelligence and machine learning systems. It documents real-world attack techniques, vulnerabilities, and mitigation specific to AI technology.