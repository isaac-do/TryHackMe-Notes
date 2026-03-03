---
date: 2026-01-28
tags:
  - dfir
  - forensics
  - mobile
---
# Level of Acquisition
---

| Acquisition Method | Description                                                                                                                                                                    | Use Case                                                                                                                                                                                                           | Level of Access                               |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------- |
| Manual             | Manual acquisition involves manually interacting with the device to gather information, such as scrolling through chat messages or taking pictures with an alternative device. | This is incredibly valuable if the device is currently unlocked, as many security mechanisms have already been bypassed.<br><br>However, this raises serious issues concerning data integrity and non-repudiation. | Minimal access to system logs and databases.  |
| Logical            | Logical acquisition involves using features of the mobile devices' Operating System (such as APIs, backup features, etc) to extract data.                                      | This method is helpful for cases where the device is locked, using another trusted (by the mobile) device to authenticate.                                                                                         | Partial.                                      |
| File System        | File System acquisition involves creating an entire copy of the device's file system.                                                                                          | It usually requires exploiting a vulnerability, MDM, jailbreaking, or using specialist toolkits to obtain privileged access to the filesystem.                                                                     | Substantial.                                  |
| Physical           | An entire bit-for-bit image of the device, allowing deleted data to be recovered.                                                                                              | Difficult with modern devices due to extensive security mechanisms; however, incredibly valuable, especially with older devices.                                                                                   | Full (on devices without encryption at rest). |
# Maintaining Access
---
Disabling the lock screen timer, which can be configured in the respective settings on Android and iOS, can be an effective way to ensure a device remains unlocked.

Additionally, it is essential to enable the "airplane" mode on mobile devices. This prevents any modification to data, and in the case of iOS, prevents remote wiping via the "Find My" feature.