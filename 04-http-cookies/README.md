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

<br>

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
- **Strict**: only to the same site that originated it
- **Lax**: same, with exception for when the user navigates to a URL from an external site
- **None**: no restrictions

<br>

**Cookie prefixes**

<br>

**JavaScript access using `Document.cookie`**


<br>
An expiration date or duration can be specified, after which the cookie is no longer sent. Additional restrictions to a specific domain and path can be set, limiting where the cookie is sent.