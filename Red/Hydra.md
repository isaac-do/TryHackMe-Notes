---
date: 2026-03-03
tags:
  - hydra
---
# What is Hydra?
---
Hydra is a brute force online password cracking program, a quick system login password “hacking” tool.

Hydra can run through a list and “brute force” some authentication services. Imagine trying to manually guess someone’s password on a particular service (SSH, Web Application Form, FTP or SNMP) - we can use Hydra to run through a password list and speed this process up for us, determining the correct password.

# Using Hydra
---
##  SSH
`hydra -l <username> -P <full path to pass> 10.66.149.65 -t 4 ssh`

| Option | Description                            |
| ------ | -------------------------------------- |
| -l     | specifies the (SSH) username for login |
| -P     | indicates a list of passwords          |
| -t     | sets the number of threads to spawn    |

## Post Web Form
`sudo hydra <username> <wordlist> 10.66.149.65 http-post-form "<path>:<login_credentials>:<invalid_response>"`

| Option                | Description                                                                             |
| --------------------- | --------------------------------------------------------------------------------------- |
| -l                    | the username for (web form) login                                                       |
| -P                    | the password list to use                                                                |
| http-post-form        | the type of form is POST                                                                |
| `<path>`              | the login page URL, for example: `login.php`                                            |
| `<login_credentials>` | the username and password used to login, for example: `username=^USER^&password=^PASS^` |
| `<invalid_response>`  | part of the response when the login fails                                               |
| -V                    | verbose output for every attempt                                                        |
**Note:** If the web server is listening on a non-default port number, you can explicitly specify the port number using `-s <port>`.