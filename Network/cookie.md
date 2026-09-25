# Cookies

**Cookie** - это небольшой фрагмент данных, который веб-сайт просит браузер сохранить.

Cookies используются для того, чтобы сайт мог **сохранять определённую информацию между запросами** и узнавать браузер при последующих запросах.

Например:

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

# Для чего используются Cookies

Cookies могут использоваться для:

* хранения сессии пользователя;
* авторизации;
* сохранения настроек сайта;
* сохранения языка;
* хранения содержимого корзины;
* аналитики;
* отслеживания определённых действий пользователя.

Например, после входа на сайт сервер может создать сессию и отправить браузеру Cookie:

```
Set-Cookie: session=abc123
```

Браузер сохранит её и будет отправлять при следующих подходящих запросах.

---

# Cookies и HTTP

HTTP является **протоколом без состояния**. (stateless-protocol)

Это означает, что каждый HTTP-запрос сам по себе не обязан содержать информацию о предыдущих запросах.

Например:

```
Request 1:
GET /login

Request 2:
GET /profile

Request 3:
GET /settings
```

Серверу необходимо каким-то образом связать эти запросы с одним пользователем.

Для этого может использоваться Cookie.

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

Для установки Cookie сервер отправляет HTTP-заголовок:

```
Set-Cookie: session=abc123
```

Например:

```
HTTP/1.1 200 OK
Set-Cookie: session=abc123
Content-Type: text/html
```

Браузер получает этот заголовок и сохраняет Cookie.

После этого браузер может отправить Cookie обратно серверу:

```
Cookie: session=abc123
```

Таким образом:

```
Server → Set-Cookie
Browser → Cookie
```
---

# Cookie и Session

Cookie и Session - **не одно и то же**.

### Cookie

Cookie - это данные, которые браузер хранит на стороне клиента.

### Session

Session - это состояние пользователя, которое обычно хранится на сервере.

Например:

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

В Cookie может находиться только идентификатор:

```
session=abc123
```

А сервер по этому идентификатору может найти соответствующую сессию.

---

# Пример авторизации

Представим, что пользователь вводит логин и пароль:

```
Username: user
Password: ********
```

Сервер проверяет данные.

Если они правильные:

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

Браузер получает:

```
Set-Cookie: session=abc123
```

Именно из-за этого каждый раз при переходе в **/profile** (и тому подобные страницы), вам не надо каждый раз вводить логин и пароль заново

---

# Cookie Attributes

Cookies могут иметь специальные атрибуты.

Например:

```
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Lax
```

В этом примере используются:

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

Атрибут `Secure` означает, что Cookie должна отправляться только через **HTTPS**.

Например:

```
Set-Cookie: session=abc123; Secure
```

Это помогает предотвратить передачу такой Cookie через обычное незашифрованное HTTP-соединение.

---

### HttpOnly

```
HttpOnly
```

Этот атрибут запрещает обычному JavaScript-коду получать Cookie через:

```
document.cookie
```

Например:

```
Set-Cookie: session=abc123; HttpOnly
```

Это особенно важно для сессионных Cookies, поскольку помогает ограничить доступ к ним из JavaScript.

`HttpOnly` не делает Cookie полностью неуязвимой и не защищает от всех атак.

---

### SameSite

```
SameSite
```

Определяет, при каких cross-site запросах браузер может отправлять Cookie.

Основные значения:

```
SameSite=Strict
SameSite=Lax
SameSite=None

* Strict - никогда не отправляет куки при переходах с внешних сайтов;

* Lax - отправляет только при безопасной навигации (например, переход по ссылке GET);

* None - отправляет всегда (требует обязательный флаг Secure).
```

Например:

```
Set-Cookie: session=abc123; SameSite=Lax
```

`SameSite` является важным механизмом защиты от некоторых атак, связанных с отправкой запросов с другого сайта, например **CSRF**.

---

# Где посмотреть Cookies

Cookies можно посмотреть через Developer Tools браузера.

Обычно:

```
Developer Tools
       ↓
Storage / Application
       ↓
    Cookies
```

Также Cookies можно увидеть в HTTP-запросах.

Например:

```
 Developer Tools
       ↓
    Network
       ↓
    Request
       ↓
    Headers
```

В запросе можно увидеть:

```
Cookie: session=abc123
```

А в ответе сервера:

```
Set-Cookie: session=abc123; HttpOnly; Secure
```

---

# Пример HTTP-взаимодействия

Первый запрос:

```
GET /login HTTP/1.1
Host: example.com
```

Сервер отвечает:

```
HTTP/1.1 200 OK
Set-Cookie: session=abc123; HttpOnly; Secure
```

Браузер сохраняет Cookie.

Следующий запрос:

```
GET /profile HTTP/1.1
Host: example.com
Cookie: session=abc123
```

Сервер получает Cookie и может определить сессию пользователя.

---

# Cookies и безопасность

Cookies особенно важны в кибербезопасности, потому что сессионные Cookies могут использоваться для поддержания авторизации.

Например:

```
Login
  ↓
Session created
  ↓
Session Cookie
  ↓
Authenticated requests
```

Если злоумышленник получит действующую сессионную Cookie, это может позволить ему использовать существующую сессию пользователя.

Поэтому сессионные Cookies необходимо защищать.

Основные меры:

* использовать HTTPS;
* использовать `Secure`;
* использовать `HttpOnly` для чувствительных сессионных Cookies;
* правильно настраивать `SameSite`;
* использовать безопасное управление сессиями;
* регулярно обновлять и инвалидировать сессии там, где это необходимо.

---

# Cookies и XSS

**XSS (Cross-Site Scripting)** - уязвимость, при которой злоумышленник может добиться выполнения JavaScript в контексте веб-сайта.

Например, без `HttpOnly` JavaScript может обращаться к Cookie через:

```
document.cookie
```

Поэтому:

```
Set-Cookie: session=abc123; HttpOnly
```

может помочь защитить сессионную Cookie от чтения через JavaScript.

Однако `HttpOnly` **не устраняет саму XSS-уязвимость**.

---

# Cookies и CSRF

**CSRF (Cross-Site Request Forgery)** - атака, при которой пользователя могут попытаться заставить отправить нежелательный запрос к сайту, где он уже авторизован.

Cookies имеют значение для CSRF, потому что браузер может автоматически отправлять подходящие Cookies вместе с запросом.

Для защиты могут использоваться:

* `SameSite`;
* CSRF tokens;
* проверка `Origin` / `Referer`;
* другие механизмы защиты.

---

# Cookie ≠ пароль

Важно понимать:

```
Cookie ≠ Password
```

Например:

```
Username + Password
        ↓
  Authentication
        ↓
     Session
        ↓
 Session Cookie
```

После авторизации браузеру обычно не нужно отправлять пароль при каждом запросе.

Вместо этого он отправляет идентификатор сессии:

```
Cookie: session=abc123
```

Поэтому сессионная Cookie может быть **очень чувствительной информацией**.

---

**Главная идея:**

> **Cookie - это небольшой фрагмент данных, который браузер хранит и может отправлять серверу вместе с последующими HTTP-запросами.**
