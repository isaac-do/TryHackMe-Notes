---
date: 2026-01-20
tags:
  - dfir
  - forensics
  - linux
  - imaging
  - disk-imaging
---
**Imaging:** creating an exact, bit-by-bit copy of digital storage media.
**Write-blockers:** a specialized computer hard disk controller made for gaining read-only access to hard drives.

# Preparation
---
## Audit Trailing Commands

| Command                         | Description                                                                         |
| ------------------------------- | ----------------------------------------------------------------------------------- |
| set -o history                  | Enables command history in the shell, allowing it to record the commands you enter. |
| shopt -s histappend             | Appends the command history to the history file instead of overwriting it.          |
| export HISTCONTROL=             | Clears any settings that control which commands are saved.                          |
| export HISTIGNORE=              | Clears any settings that ignore specific patterns of commands.                      |
| export HISTFILE=~/.bash_history | Sets the file where the command history is saved.                                   |
| export HISTFILESIZE=-1          | Sets no limit on the number of lines stored in the history file.                    |
| export HISTSIZE=-1              | Sets no limit on the number of commands retained in the shell history.              |
| export HISTTIMEFORMAT="%F-%R "  | Formats timestamps in the history as "YYYY-MM-DD HH" for each command.              |
## Accessing the File System
`/dev/root` is the current disk used by the OS.
Drives are attached as devices under the `/dev` directory.
Virtual Disks attached to `loop interfaces` will not appear under the `df` command.
- *Also common for physical disks when not directly attached.*
- Use `lsblk -a` command to solve this.

Use the `losetup -l <device>` command to list the device and assigned interface path.
Example:
```bash
sudo losetup -l /dev/loop11
```

Use the `blkid <device>` command to list the UUID of the image.
Example:
```bash
sudo blkid /dev/loop11
```

# Creating a Forensic Image
---
## Imaging
To start imaging, run the command `dc3dd if=<input_file> of=<output_file> log=<log_file>`.
Example:
```bash
sudo dc3dd if=/dev/loop11 of=example1.img log=imaging_loop11.txt
```

## Integrity Checking
We must ensure that the image we created is identical to the original. To verify, we can compare the hash values of both the copy and the original.

To check the hash of a file/folder, run the command `sudo md5sum <file>`.
Example:
```bash
sudo md5sum example1.img
483ca14c7524b8667974a922662b87e8  example1.img
sudo md5sum /dev/loop11
483ca14c7524b8667974a922662b87e8  /dev/loop11
```

# Inspect The Image
---
To inspect the image, we can mount it.

First, confirm with the `df -h` command that the disk is not mounted.
Then do the following steps to mount the image:
- `sudo mkdir -p <target>`
	- The `-p` will create the directory if none exist and will not throw errors.
- `sudo mount -o <option> <source> <target>`
	- The `-o` is an options switch. Using `-o loop` treats the `<source>` as a disk device.

Example:
```bash
sudo mkdir -p /mnt/example1
sudo mount -o loop example1.img /mnt/example1
```
