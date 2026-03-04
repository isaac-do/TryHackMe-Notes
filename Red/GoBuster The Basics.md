---
date: 2026-03-03
tags:
  - gobuster
  - offensive-security
---
# Introduction
---
Gobuster is an open-source offensive tool that enumerates web directories, DNS subdomains, vhosts, Amazon S3 buckets, and Google Cloud Storage by brute force, using specific wordlists and handling the incoming responses. 

Example Usage:
```shell
gobuster dir -u "http://www.example.thm/" -w /usr/share/wordlists/dirb/small.txt -t 64
```
- `-u "http://www.example.thm/"` tells Gobuster that the target URL is `http://example.thm/`.
- `-w /usr/share/wordlists/dirb/small.txt` directs Gobuster to use the small.txt wordlist to brute force the web directories.
- `-t 64` sets the number of threads Gobuster will use to 64.

Common Flags:

| Short Flag | Long Flag  | Description                                                                                                                                                                                                                                                                                                            |
| ---------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -t         | --threats  | This flag configures the number of threads to use for the scan. Each of these threads sends out requests with a slight delay. The default number of threads is 10. This number may be slow when using large wordlists. You can increase or decrease the number of threads depending on the available system resources. |
| -w         | --wordlist | The flag configures a wordlist to use for iterating. Each wordlist entry is attached to the URL you included in the command.                                                                                                                                                                                           |
|            | --delay    | This flag defines the amount of time to wait between sending requests. Some web servers include mechanisms to detect enumeration by looking at how many requests are received in a certain period of time. We can increase the delay between subsequent requests to make it look like normal web traffic.              |
|            | --debug    | This flag helps us to troubleshoot when our command gives unexpected errors.                                                                                                                                                                                                                                           |
| -o         | --output   | This flag writes the enumeration results to a file we choose.                                                                                                                                                                                                                                                          |

# Use Case: Directory and File Enumeration
---
Example Usage:
```shell
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r
```
- `gobuster dir`: Configures Gobuster to use the directory and file enumeration mode.
- `-u http://www.example.thm`: The URL will be the base path where Gobuster starts looking. So, the URL  above is using the root web directory.
- `-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` configures Gobuster to use the directory-list-2.3-medium.txt wordlist to enumerate. Each entry of the wordlist is appended to the configured URL.
- `-r` configures Gobuster to follow the redirect responses received from the sent requests. If a status code 301 was received, Gobuster will navigate to the redirect URL that is included in the response.

Add the `-x` flag to specify what type of files we want to enumerate:
```shell
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js
```

Common Flags:

| Flag | Long Flag                | Description                                                                                                                                                                                                 |
| ---- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -c   | --cookie                 | This flag configures a cookie to pass along each request, such as a session ID.                                                                                                                             |
| -x   | --exentions              | This flag specifies which file extensions you want to scan for. E.g., .php, .js                                                                                                                             |
| -H   | --headers                | This flag configures an entire header to pass along with each request.                                                                                                                                      |
| -k   | --no-tls-validation      | This flag  skips the process that checks the certificate when https is used. This causes an error during the TLS check.                                                                                     |
| -n   | --no-status              | You can set this flag when you don’t want to see status codes of each response received. This helps keep the output on the screen clear.                                                                    |
| -P   | password                 | You can set this flag together with the `--username` flag to execute authenticated requests. This is handy when you have obtained credentials from a user.                                                  |
| -s   | --status-codes           | With this flag, you can configure which status codes of the received responses you want to display, such as 200, or a range like 300-400.                                                                   |
| -b   | --status-codes-blacklist | This flag allows you to configure which status codes of the received responses you don’t want to display. Configuring this flag overrides the -s flag.                                                      |
| -U   | --username               | You can set this flag together with the `--password` flag to execute authenticated requests. This is handy when you have obtained credentials from a user.                                                  |
| -r   | --followredirect         | This flags configures Gobuster to follow the redirect that it received as a response to the sent request. A HTTP redirect status code (e.g., 301 or 302) is used to redirect the client to a different URL. |

# Use Case: Subdomain Enumeration
---
Example Usage:
```shell
gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```
- `gobuster dns` enumerates subdomains on the configured domain.
- `-d example.thm` sets the target to the example.thm domain.
- `-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt` sets the wordlist to subdomains-top1million-5000.txt. Gobuster uses each entry of this list to construct a new DNS query.

Common Flags:

| Flag | Long Flag    | Description                                                                       |
| ---- | ------------ | --------------------------------------------------------------------------------- |
| -c   | --show-cname | Show CNAME Records (cannot be used with the `-i` flag).                           |
| -i   | --show-ips   | Including this flag shows IP addresses that the domain and subdomains resolve to. |
| -r   | --resolver   | This flag configures a custom DNS server to use for resolving.                    |
| -d   | --domain     | This flag configures the domain you want to enumerate.                            |

# Use Case: Vhost Enumeration
---
`vhost` mode allows Gobuster to brute force virtual hosts. Virtual hosts are different websites on the same machine.

difference between `vhost` and `dns` mode is in the way Gobuster scans:
- `vhost` mode will navigate to the URL created by combining the configured HOSTNAME (-u flag) with an entry of a wordlist.
- `dns` mode will do a DNS lookup to the FQDN created by combining the configured domain name (-d flag) with an entry of a wordlist.


What GoBuster sends when using vhost: 
```http
GET / HTTP/1.1
Host: www.example.thm
User-Agent: gobuster/3.6
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
```

Example Usage:
```shell
gobuster vhost -u "http://MACHINE_IP" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320 
```
- `gobuster vhost` instructs Gobuster to enumerate virtual hosts.
- `-u "http://MACHINE_IP"` sets the URL to browse to MACHINE_IP.
- `-w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt` configures Gobuster to use the _subdomains-top1million-5000.txt_ wordlist. Gobuster appends each entry in the wordlist to the configured domain. If no domain is explicitly configured with the `--domain` flag, Gobuster will extract it from the URL. E.g., _test.example.thm_, _help.example.thm_, etc. If any subdomains are found, Gobuster will report them to you in the terminal. 
- `--domain example.thm` sets the top- and second-level domains in the `Hostname:` part of the request to _example.thm._  
- `--append-domain` appends the configured domain to each entry in the wordlist. If this flag is not configured, the set hostname would be _www_, _blog_, etc. This will cause the command to work incorrectly and display false positives.
- `--exclude-length` filters the responses we get from the sent web requests. With this flag, we can filter out the false positives. If you run the command without this flag, you will notice you will get a lot of false positives like "Found: Orion.example.thm Status: 404 [Size: 279]" or  "Found: pm.example.thm Status: 404 [Size: 276]". These false positives typically have a similar response size, so we can use this to filter out most false positives. We expect to get a 200 OK response back to have a true positive.

Common Flags:

| Flag | Long Flag         | Description                                                                                           |
| ---- | ----------------- | ----------------------------------------------------------------------------------------------------- |
| -u   | --url             | Specifies the base URL (target domain) for brute-forcing virtual hostnames.                           |
|      | --append-domain   | Appends the base domain to each word in the wordlist (e.g., word.example.com).                        |
| -m   | --method          | Specifies the HTTP method to use for the requests (e.g., GET, POST).                                  |
|      | --domain          | Appends a domain to each wordlist entry to form a valid hostname (useful if not provided explicitly). |
|      | --exclude-length  | Excludes results based on the length of the response body (useful to filter out unwanted responses).  |
| -r   | --follow-redirect | Follows HTTP redirects (useful for cases where subdomains may redirect).                              |
