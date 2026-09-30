# HTTP and HTTPS

## HTTP

**HTTP (Hypertext Transfer Protocol)** is a protocol that enables the client and server to exchange data over the internet.

Example:

```
Browser → HTTP Request → Server
Browser ← HTTP Response ← Server
```

HTTP works on top of a transport protocol, usually **TCP**

Standard port:

```
HTTP → TCP 80
```

---

## HTTP Request

**HTTP Request** is a request that the client sends to the server

Example:

```
GET /profile HTTP/1.1
Host: example.com
User-Agent: Firefox
```

Main parts:

* `GET` - HTTP method;
* `/profile` - path to the resource;
* `HTTP/11` - HTTP version;
* `Host` - server domain;
* `User-Agent` - client information

### Main HTTP methods

| Method | Purpose |
| -------- | -------------------------  |
| `GET`    | Get data                   |
| `POST`   | Send data                  |
| `PUT`    | Completely update resource |
| `PATCH`  | Partially modify resource  |
| `DELETE` | Delete resource            |

---

## HTTP Response

**HTTP Response** - the server’s response to an HTTP request

Example:

```
HTTP/1.1 200 OK
Content-Type: text/html
```

After the headers, the server can send the response body:

```
<html>
    
</html>
```

### Main HTTP Status Codes

| Code                         | Meaning                   |
| --------------------------- | -------------------------- |
| `200 OK`                    | Request successfully completed |
| `301`                       | Permanent redirect         |
| `302`                       | Temporary redirect         |
| `400 Bad Request`           | Invalid request            |
| `401 Unauthorized`          | Authentication required     |
| `403 Forbidden`             | Access denied              |
| `404 Not Found`             | Resource not found          |
| `500 Internal Server Error` | Server Error             |

---

# HTTPS

**HTTPS (HTTP Secure)** - HTTP running over a secure TLS connection

Simplified:

```
HTTP + TLS = HTTPS
```

Standard port:

```
HTTPS → TCP 443
```

Example:

```
https://example.com
```

---

## TLS

**TLS (Transport Layer Security)** is a cryptographic protocol that protects the connection between a client and a server.

TLS provides:

### 1 Confidentiality - confidentiality

Data is encrypted, so an unauthorized party should not be able to simply read the contents of the traffic.

### 2 Integrity - integrity

TLS helps detect data changes during transmission.

### 3 Authentication - authentication

A TLS certificate allows a browser to verify the authenticity of a server and its association with a domain.

---

## TLS Certificate

With HTTPS, the server provides a **TLS certificate**.

In simplified terms:

```
example.com
     ↓
TLS Certificate
     ↓
Browser verification
     ↓
Secure connection
```

---

# Differences between HTTP and HTTPS

| HTTP                                       | HTTPS                                        |
| ------------------------------------------ | -------------------------------------------- |
| TCP 80                                     | TCP 443                                      |
| No TLS encryption                         | Uses TLS                               |
| Data is transmitted without TLS protection | Data is protected by TLS                          |
| Does not provide TLS server authentication | Uses TLS certificates                   |
| Less secure for sensitive data  | Used for secure web communication |

---

# HTTP and HTTPS on the web

When a user opens:

```
https://example.com
```

the simplified sequence looks like this:

```
Browser
   ↓
  DNS
   ↓
IP address
   ↓
TCP connection
   ↓
  TLS
   ↓
 HTTPS
   ↓
  GET /
   ↓
Web Server
   ↓
Application
   ↓
Database
```

From the perspective of the network model:

```
Application: HTTP / HTTPS
Transport:   TCP
Network:     IP
```
Link:        Ethernet / Wi-Fi
```

---

# HTTPS and cybersecurity

HTTPS protects the **connection**, but it does not guarantee the absence of vulnerabilities in the web application itself.

For example, a website may use HTTPS and still have:

* XSS;
* SQL Injection;
* IDOR / Broken Access Control;
* CSRF;
* SSRF;
* API vulnerabilities;
* issues with authentication and sessions.

```
Therefore:

```

HTTPS ≠ a secure web application

```

HTTPS protects data transmission between the client and the server, while the security of the application itself also depends on its architecture, code, authentication, authorization, and user data processing.

## Key points

> **HTTP** is a protocol for exchanging data between a client and a server.

> **HTTPS** is HTTP secured with TLS.

> **HTTP → TCP 80**

> **HTTPS → TCP 443**

> **HTTPS protects the connection but does not eliminate vulnerabilities in the web application itself.**
