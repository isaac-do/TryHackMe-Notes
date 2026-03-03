---
date: 2026-03-02
tags:
  - web
---
# Uniform Resource Locator (URL)
---
![attachment-03022026-10](../PNG%20PDF%20JPEG/attachment-03022026-10.png)

# HTTP Request: Request Line and Methods
---
![../PNG PDF JPEG/attachment-03022026-11.png](../PNG%20PDF%20JPEG/attachment-03022026-11.png)
**Request Line:** GET /user/login.html HTTP/1.1

# HTTP Request: Headers and Body
---

| Request Header                                           | Example                                                                        | Description                                                                              |
| -------------------------------------------------------- | ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| User-Agent                                               | User-Agent: Mozilla/5.0                                                        | Shares information about the web browser the request is coming from.                     |
| Referer                                                  | `Referer: https://www.google.com/`                                             | Indicates the URL from which the request came from.                                      |
| Cookie                                                   | Cookie: user_type=student; room=introtowebapplication; room_status=in_progress | Information the web server previously asked the web browser to store is held in cookies. |
| Content-Type                                             | Content-Type: application/json                                                 | Describes what type or format of data is in the request.                                 |

## Request Body
**URL Encoded (application/x-www-form-urlencoded)**
Example
```http
POST /profile HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 33

name=Aleksandra&age=27&country=US
```

**Form Data (multipart/form-data)**
Example
```http
POST /upload HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="username"

aleksandra
----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="profile_pic"; filename="aleksandra.jpg"
Content-Type: image/jpeg

[Binary Data Here representing the image]
----WebKitFormBoundary7MA4YWxkTrZu0gW--
```

**JSON (application/json)**
Example: 
```http
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/json
Content-Length: 62

{
    "name": "Aleksandra",
    "age": 27,
    "country": "US"
}
```


**XML (application/xml)**
Example: 
```http
POST /api/user HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Content-Type: application/xml
Content-Length: 124

<user>
    <name>Aleksandra</name>
    <age>27</age>
    <country>US</country>
</user>
```

# HTTP Response: Status Line and Status Codes
---
**Status Codes and Reason Phrases**
- Informational Responses (100-199)
	- These codes mean the server has received part of the request and is waiting for the rest. It’s a "keep going" signal.
- Successful Responses (200-299)
- Redirection Messages (300-399)
- Client Error Responses (400-499)
- Server Error Responses (500-599)

**Common Status Codes**

| Code                        | Description                                                                                             |
| --------------------------- | ------------------------------------------------------------------------------------------------------- |
| 100 (Continue)              | The server got the first part of the request and is ready for the rest.                                 |
| 200 (OK)                    | The request was successful, and the server is sending back the requested resource.                      |
| 301 (Moved Permanently)     | The resource you're request has been permanently moved to a new URL. Use the new URL from now on.       |
| 404 (Not Found)             | The server couldn't find the resource at the given URL. Double-check that you've got the right address. |
| 500 (Internal Server Error) | Something went wrong on the server's end, and it couldn't process your request.                         |

# HTTP Response: Headers and Body
---
**Required Response Headers**
- Date
	- Ex: `Date: Fri, 23 Aug 2024 10:43:21 GMT`
- Content-Type
	- Ex: `Content-Type: text/html; charset=utf-8`
	- Tells the client what kind of content it's getting (HTML, JSON, etc).
- Server
	- Ex: Server: `nginx`
	- Shows what kind of server software is handling the request.

**Other Common Response Headers**
- Set-Cookie
	- Ex: `Set-Cookie: sessionId=38af1337es7a8`
	- Sends cookies from server to client for client to store for future requests.
	- Use **HttpOnly** flag and **Secure** flag to keep cookies secure.
- Cache-Control
	- Ex: `Cache-Control: max-age=600`
	- Tells the client how long it can cache the response before checking with the server again.
- Location
	- Ex: `Location: /index.html`
	- Tells the client where to go next if the resource has moved.

# Security Headers
---
## Content-Security-Policy (CSP)
Provides additional security layer to protect against common attacks like XSS.
Provides a way for admins to specify what domains or sources are considered safe.

Example CSP Header:
```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.tryhackme.com; style-src 'self'
```

- **default-src:**  specifies the default policy of self, which means only the current website.
- **script-src:** specifics the policy for where scripts can be loaded from, which is self along with scripts hosted on `https://cdn.tryhackme.com`
- **style-src:**  specifies the policy for where style CSS style sheets can be loaded from the current website (self).

## Strict-Transport-Security (HSTS)
This header ensures that web browsers always connect over HTTPS.

Example HSTS:
```http
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

- **max-age:** expiry time in seconds.
- **includeSubDomains:** optional setting that instructs the browser to also apply this setting to all subdomains.
- **preload:** allows the website to be included in the preload lists.

## X-Content-Type-Options
This header instruct browsers to not guess the MIME type of a resource but only use the Content-Type header.

Example:
```http
X-Content-Type-Options: nosniff
```
- **nosniff:** instructs the browser to not sniff or guess the MIME type.

## Referrer-Policy
This header controls the amount of information sent to the destination web server when a user is redirected from the source web server.

Some examples of Referrer-Policy:
```http
Referrer-Policy: no-referrer
Referrer-Policy: same-origin
Referrer-Policy: strict-origin
Referrer-Policy: strict-origin-when-cross-origin
```
- **no-referrer:** disables any information being sent about the referrer.
- **same-origin:** only send referrer information when the destination is part of the same origin.
- **strict-origin:** only sends the referrer as the origin when the protocol stays the same.
	- A referrer sent when using an HTTPS connection goes to another HTTPS connection.
- **strict-origin-when-cross-origin:** similar to strict-origin except it sends the full URL path in the origin header.