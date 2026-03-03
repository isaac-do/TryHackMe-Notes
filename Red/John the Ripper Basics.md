---
date: 2026-02-10
tags:
  - johntheripper
  - hashing
  - cryptography
---
# Cracking Basic Hashes
---
## John Basic Syntax
`john [options] [file path]`
- john: invokes the John the Ripper program.
- `[options]` specifies the options you want to use.
- `[file path]` the file containing the hash you're trying to crack.

## Automatic Cracking 
`john --wordlist=[path to wordlist] [path to file]`
- `--wordlist=` specifies using wordlist mode, reading from the file supplied.
Example Usage:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

## Format-Specific Cracking
Once hash is identified:
`john --format=[format] --wordlist=[path to wordlist] [path to file]`
- `--format=` flag that tells John you're giving it a hash specific format.

**Note:** some standard hash types (e.g. md5) requires the `raw-` prefix.
To check if a prefix is needed or not, run `john --list=formats` then using `| grep -iF "<HASH_TYPE>"`.

Command Order:
1. `john --format=[format] --wordlist=[path to wordlist] [path to file]`
2. `john --show --format=[format] [path to file]`

# Cracking /etc/shadow Hashes
---
To crack `/etc/shadow` passwords, combine it with the `/etc/passwd` file for John to understand the data it's being given.
- Use the tool `unshadow` from John's suite of tools.

## unshadow Basic Syntax
`unshadow [path to passwd] [path to shadow]`
Example Usage:
```bash
unshadow local_passwd local_shadow > unshadowed.txt
```

Command Order:
1. `unshadow local_passwd local_shadow > unshadowed.txt`
2. `john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt`

# Single Crack Mode
---
## Word Mangling
Consider the username “Markus”.

Some possible passwords could be:

    Markus1, Markus2, Markus3 (etc.)
    MArkus, MARkus, MARKus (etc.)
    Markus!, Markus$, Markus* (etc.)

This technique is called word mangling.

## GECOS
Fields separated by the colon `:` in the entries for both `/etc/shadow` and `/etc/passwd`. The fifth field in the user account record is the GECOS field.

# Using Single Crack Mode
Basic Syntax: `john --single --format=[format] [path to file]`
- `--single`: This flag lets John know you want to use the single hash-cracking mode
Example Usage:
```bash
john --single --format=raw-sha256 hashes.txt
```

## A Note on File Formats in Single Crack Mode:
If you’re cracking hashes in single crack mode, you need to change the file format that you’re feeding John for it to understand what data to create a wordlist from. You do this by prepending the hash with the username that the hash belongs to, so according to the above example, we would change the file `hashes.txt`.

**From** `1efee03cdcb96d90ad48ccc7b8666033`
**To** `mike:1efee03cdcb96d90ad48ccc7b8666033`

# Custom Rules
---
Custom rules are defined in the `john.conf` file.
It is usually located in `/etc/john/john.conf` if you have installed John using a package manager or built from source with `make`.

`[List.Rules:THMRules]` is used to define the name of your rule; this is what you will use to call your custom rule a John argument.
Example Usage:
```
[List.Rules:PoloPassword]
cAz"[0-9] [!£$%@]"
```

We could then call this custom rule a John argument using the  `--rule=PoloPassword` flag.

# Cracking Password Protected Zip Files
---
We can use John to crack the password on password-protected Zip files.

## Zip2John
Use the `zip2john` tool to convert the Zip file into a hash format that John can understand and hopefully crack.

Basic Syntax:
`zip2john [options] [zip file] > [output file]`
- `[options]`: Allows you to pass specific checksum options to `zip2john`; this shouldn’t often be necessary
- `[zip file]`: The path to the Zip file you wish to get the hash of
- `>`: This redirects the output from this command to another file
- `[output file]`: This is the file that will store the output
Example Usage:
```bash
zip2john zipfile.zip > zip_hash.txt
```

Command Order:
1. `zip2john zipfile.zip > zip_hash.txt`
2. `john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt`
3. `john --show zip_hash.txt`

# Cracking Password-Protected RAR Archives
---
Almost identical to the `zip2john` tool, we will use the `rar2john` tool to convert the RAR file into a hash format that John can understand.

Basic Syntax:
`rar2john [rar file] > [output file]`
- `rar2john`: Invokes the `rar2john` tool
- `[rar file]`: The path to the RAR file you wish to get the hash of
- `>`: This redirects the output of this command to another file
- `[output file]`: This is the file that will store the output from the command
Example Usage:
```bash
/opt/john/rar2john rarfile.rar > rar_hash.txt
```

# Cracking SSH Keys with John
---
`ssh2john` converts the `id_rsa` private key, which is used to log in to the SSH session, into a hash format that John can work with.

Basic Syntax:
`ssh2john [id_rsa private key file] > [output file]`
- `ssh2john`: Invokes the `ssh2john` tool
- `[id_rsa private key file]`: The path to the id_rsa file you wish to get the hash of
- `>`: This is the output director. We’re using it to redirect the output from this command to another file.
- `[output file]`: This is the file that will store the output from
Example Usage:
```bash
/opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
```
