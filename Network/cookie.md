# Cookies

**Cookie** is a small piece of data that a website asks the browser to store.

Cookies are used so that a website can **store certain information between requests** and recognize the browser in subsequent requests.

For example:

```
Browser                         Server
   │                               │
   │────── HTTP request ──────────>│
   │                               │
   │<──── Set-Cookie: session ─────│
   │                               │
   │       Cookie saved            │
   │                               │
   │──── Cookie: session=abc123 ──>│
   │                               │
```

---

# What Cookies Are Used For

Cookies can be used for:

* storing a user's session;
* authentication;
* saving website preferences;
* storing language preferences;
* storing shopping cart contents;
* analytics;
* tracking certain user actions.

For example, after logging into a website, the server may create a session and send a Cookie to the browser:

```
Set-Cookie: session=abc123
```

The browser stores it and sends it with subsequent applicable requests.

---

# Cookies and HTTP

HTTP is a **stateless protocol**.

This means that each HTTP request does not inherently have to contain information about previous requests.

For example:

```
Request 1:
GET /login

Request 2:
GET /profile

Request 3:
GET /settings
```

The server needs some way to associate these requests with the same user.

A Cookie can be used for this purpose.

```
Login
  ↓
Server creates session
  ↓
Set-Cookie: session=abc123
  ↓
Browser stores Cookie
  ↓
Browser sends Cookie with requests
```

---

# Set-Cookie

To set a Cookie, the server sends an HTTP header:

```
Set-Cookie: session=abc123
```

For example:

```
HTTP/1.1 200 OK
Set-Cookie: session=abc123
Content-Type: text/html
```

The browser receives this header and stores the Cookie.

After that, the browser can send the Cookie back to the server:

```
Cookie: session=abc123
```

So:

```
Server → Set-Cookie
Browser → Cookie
```

---

# Cookie and Session

Cookie and Session are **not the same thing**.

### Cookie

A Cookie is data that the browser stores on the client side.

### Session

A Session is the state associated with a user, which is usually stored on the server.

For example:

```
Browser
   │
   │ Cookie: session=abc123
   ↓
Server
   │
   │ abc123 → User #42
   ↓
Session storage
```

The Cookie may contain only an identifier:

```
session=abc123
```

The server can then use this identifier to find the corresponding session.

---

# Authentication Example

Suppose a user enters their username and password:

```
Username: user
Password: ********
```

The server verifies the credentials.

If they are correct:

```
Login
  ↓
Authentication successful
  ↓
Create session
  ↓
session=abc123
  ↓
Send Cookie to browser
```

The browser receives:

```
Set-Cookie: session=abc123
```

Because of this, you do not have to enter your username and password again every time you visit **/profile** or similar pages.

---

# Cookie Attributes

Cookies can have special attributes.

For example:

```
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Lax
```

In this example, the following attributes are used:

```
Secure
HttpOnly
SameSite
```

---

### Secure

```
Secure
```

The `Secure` attribute means that the Cookie should only be sent over **HTTPS**.

For example:

```
Set-Cookie: session=abc123; Secure
```

This helps prevent the Cookie from being transmitted over an ordinary unencrypted HTTP connection.

---

### HttpOnly

```
HttpOnly
```

This attribute prevents regular JavaScript code from accessing the Cookie through:

```
document.cookie
```

For example:

```
Set-Cookie: session=abc123; HttpOnly
```

This is especially important for session Cookies because it helps limit access to them from JavaScript.

`HttpOnly` does not make a Cookie completely secure and does not protect against all attacks.

---

### SameSite

```
SameSite
```

Controls when a browser may send a Cookie in cross-site requests.

The main values are:

```
SameSite=Strict
SameSite=Lax
SameSite=None
```

* **Strict** — the Cookie is generally not sent with cross-site requests;
* **Lax** — the Cookie can be sent in certain top-level navigations, such as following a link using `GET`;
* **None** — the Cookie can be sent in cross-site contexts; it requires the `Secure` attribute.

For example:

```
Set-Cookie: session=abc123; SameSite=Lax
```

`SameSite` is an important security mechanism that helps protect against certain attacks involving cross-site requests, such as **CSRF**.

---

# Where to View Cookies

Cookies can be viewed through the browser's Developer Tools.

Usually:

```
Developer Tools
       ↓
Storage / Application
       ↓
    Cookies
```

Cookies can also be viewed in HTTP requests.

For example:

```
Developer Tools
       ↓
    Network
       ↓
    Request
       ↓
    Headers
```

In a request, you may see:

```
Cookie: session=abc123
```

And in a server response:

```
Set-Cookie: session=abc123; HttpOnly; Secure
```

---

# HTTP Interaction Example

First request:

```
GET /login HTTP/1.1
Host: example.com
```

The server responds:

```
HTTP/1.1 200 OK
Set-Cookie: session=abc123; HttpOnly; Secure
```

The browser stores the Cookie.

The next request:

```
GET /profile HTTP/1.1
Host: example.com
Cookie: session=abc123
```

The server receives the Cookie and can identify the user's session.

---

# Cookies and Security

Cookies are especially important in cybersecurity because session Cookies can be used to maintain authentication.

For example:

```
Login
  ↓
Session created
  ↓
Session Cookie
  ↓
Authenticated requests
```

If an attacker obtains a valid session Cookie, they may be able to use the existing user session.

Therefore, session Cookies must be properly protected.

Main security measures include:

* use HTTPS;
* use `Secure`;
* use `HttpOnly` for sensitive session Cookies;
* configure `SameSite` correctly;
* use secure session management;
* regularly rotate and invalidate sessions where appropriate.

---

# Cookies and XSS

**XSS (Cross-Site Scripting)** is a vulnerability where an attacker can cause JavaScript to execute in the context of a website.

For example, without `HttpOnly`, JavaScript may be able to access a Cookie through:

```
document.cookie
```

Therefore:

```
Set-Cookie: session=abc123; HttpOnly
```

can help protect a session Cookie from being read by JavaScript.

However, `HttpOnly` **does not remove the XSS vulnerability itself**.

---

# Cookies and CSRF

**CSRF (Cross-Site Request Forgery)** is an attack where a user may be tricked into sending an unwanted request to a website where they are already authenticated.

Cookies are relevant to CSRF because browsers may automatically send applicable Cookies along with requests.

Protection mechanisms can include:

* `SameSite`;
* CSRF tokens;
* checking `Origin` / `Referer`;
* other security mechanisms.

---

# Cookie ≠ Password

It is important to understand:

```
Cookie ≠ Password
```

For example:

```
Username + Password
        ↓
  Authentication
        ↓
     Session
        ↓
 Session Cookie
```

After authentication, the browser usually does not need to send the password with every request.

Instead, it sends the session identifier:

```
Cookie: session=abc123
```

Therefore, a session Cookie can be **very sensitive information**.

---

# Main Idea

> **A Cookie is a small piece of data that a browser stores and may send to a server along with subsequent HTTP requests.**
