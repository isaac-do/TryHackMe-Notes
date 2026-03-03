---
date: 2026-02-13
tags:
  - dfir
  - windows
---

# Windows Event Logs Analysis
---
Some of the crucial types of logs stored in a Windows Operating System are:
- **Application:** There are many applications running on the operating system. Any information related to those applications is logged into this file. This information includes errors, warnings, compatibility issues, etc.
- **System:** The operating system itself has different running operations. Any information related to these operations is logged in the System log file. This information includes driver issues, hardware issues, system startup and shutdown information, services information, etc.
- **Security:** This is the most important log file in Windows OS in terms of security. It logs all security-related activities, including user authentication, changes in user accounts, security policy changes, etc.

Here is a table of some important Event IDs in Windows Operating System.

| Event ID | Description                                        |
| -------- | -------------------------------------------------- |
| 4624     | A user account successfully logged in              |
| 4625     | A user account failed to login                     |
| 4634     | A user account successfully logged off             |
| 4720     | A user account successfully logged off             |
| 4720     | A user account was created                         |
| 4724     | An attempt was made to reset an account’s password |
| 4722     | A user account was enabled                         |
| 4725     | A user account was disabled                        |
| 4726     | A user account was deleted                         |
