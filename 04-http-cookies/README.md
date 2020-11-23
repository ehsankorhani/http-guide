# HTTP Cookies

Cookies are typically, used to tell if two requests came from the same browser.

cookie is a small piece of data that a server sends to the user's web browser. The browser will store the cookie and might send it back with later requests to the same server. 

They are mainly used for:

- **Session management**
Logins, shopping carts, game scores, or anything else the server should remember.
- **Personalization**
User preferences, themes, and other settings
- **Tracking**
Recording and analyzing user behavior

<br>

Cookies are no longer used for client-side storage, instead we should use:
- Web Storage API
    - `sessionStorage`: stores data only for a session, meaning that the data is stored until the browser or tab is closed.
    - `localStorage`: persists even when the browser is closed.


- `IndexedDB`<br>
    To store and retrieve objects that are indexed with a key

<br><br>
---
## Creating cookies

After receiving an HTTP request, a server can send one or more `Set-Cookie` headers with the response.

```
Set-Cookie: <cookie-name>=<cookie-value>
```
For example:

```HTTP/2.0 200 OK
Content-Type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```
Tells the client to store a pair of cookies.

<br>

With every subsequent request, the browser sends back all previously stored cookies to the server using the `Cookie` header.

```
GET /sample_page.html HTTP/2.0
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

<br>

### Define the lifetime of a cookie

This can be defined in two ways:

- *Session* cookies are deleted when the current session ends. The browser defines when the "current session" ends.
- *Permanent* cookies are deleted at a date specified by the `Expires` attribute, or after a period of time specified by the `Max-Age` attribute.

```
Set-Cookie: id=a3fWa; Expires=Thu, 31 Oct 2021 07:28:00 GMT;
```

<br>

### Restrict access to cookies

There are a few ways to ensure that cookies are sent securely and are not accessed by unintended parties or scripts

- `Secure`<br>
    is sent to the server only over the **HTTPS** protocol.
- `HttpOnly`<br>
    is inaccessible to the JavaScript `Document.cookie` API. This helps mitigate cross-site scripting (XSS) attacks.

```
Set-Cookie: id=a3fWa; Expires=Thu, 21 Oct 2021 07:28:00 GMT; Secure; HttpOnly
```

<br>

### Define where cookies are sent

<br>

**Domain attribute**

Specifies which hosts are allowed to receive the cookie.

For example:
```
Domain=mozilla.org
```
cookies are available on subdomains like `developer.mozilla.org`.

If unspecified, it defaults to the *same origin* that set the cookie, *excluding subdomains*.

<br>

**Path attribute**

Indicates a URL path that must exist in the requested URL in order to send the Cookie header.


For example:
```
Path=/docs
```

match:
- /docs
- /docs/Web/
- /docs/Web/HTTP

<br>

**SameSite attribute**

A cookie shouldn't be sent with cross-origin requests. It provides some protection against cross-site request forgery attacks (CSRF).

```
Set-Cookie: mykey=myvalue; SameSite=Strict
```

It takes three possible values:
- `Strict`: only to the same site that originated it
- `Lax`: same, with exception for when the user navigates to a URL from an external site
- `None`: no restrictions

<br>

> Browsers are migrating to have cookies default to `SameSite=Lax`.

<br>

**Cookie prefixes**
It is possible to use cookie prefixes to assert specific facts about the cookie.

- `__Host-`: domain-locked
- `__Secure-`

Cookies with these prefixes that are not compliant with their restrictions are rejected by the browser.

<br>

**JavaScript access using `Document.cookie`**

Cookies created via JavaScript cannot include the `HttpOnly` flag.

```js
document.cookie = "yummy_cookie=choco"; 
document.cookie = "tasty_cookie=strawberry"; 
```

<br><br>

---
## Security

> Information should be stored in *cookies* with the understanding that all cookie values are visible to, and can be changed by, the end-user.

To mitigate attacks:

- Use the `HttpOnly` attribute to prevent access to cookie values via JavaScript.
- Cookies that are used for sensitive information (such as indicating authentication) should have a **short lifetime**, with the `SameSite` attribute set to `Strict` or `Lax`. 

<br><br>

---
## Tracking and privacy

<br>

### Third-party cookies
- **First-party** cookie: If `Domain` is the same as the domain of the page you are on.
- **Third-party** cookie: anything else
    - images or other components on servers, which may set third-party cookies.
    - mainly used for advertising and tracking across the web.


<br><br>

---
## `Set-Cookie` header in server-side applications

### Node.js

#### `response.setHeader(name, value)`
Sets a single header value for headers object. 

```js
request.setHeader('Cookie', ['type=ninja', 'language=javascript']);
```


<br><br>

---
### References

- [Node.js Documentation](https://nodejs.org/docs/latest-v13.x/api/http.html#http_request_setheader_name_value)
