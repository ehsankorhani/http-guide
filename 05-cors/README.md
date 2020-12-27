# Cross-Origin Resource Sharing (CORS)

Is a browser mechanism which allows accessing a resources located outside of a given domain.

However, it also provides potential for cross-domain based attacks, if a website's CORS policy is poorly configured and implemented.

Web pages you visit make frequent requests to load assets like images, fonts, and more, from many different places across the Internet. If these requests for assets go unchecked, the security of your browser may be at risk.

<br>

## Same-origin policy (SOP)
Is a web browser security mechanism that restricts scripts on one origin from accessing data from another origin.

Under this policy, a document (i.e., like a web page) hosted on server `domain-a.com` can only interact with other documents that are also on server `domain-a.com`.

![CORS Principle](./img/cors_principle.png "CORS Principle")

<br>

For security reasons, browsers restrict cross-origin HTTP requests initiated from scripts - unless the response from other origins includes the right CORS headers.

Without the same-origin policy, if you visited a malicious website, it would be able to read your emails from GMail, private messages from Facebook, etc. Because the browser stores any cookies, including authentication session cookies, relevant to the other domain.

<br>

This is a Browser only restriction.

<br>

## Why CORS?
SOP is a bit too restrictive. CORS manages cross-origin requests. In the previous example navigation from `domain-a.com` to `domain-b.com` is allowed with CORS.

CORS allows servers to specify which origins can access the assets on the server.

<br>

## How it works in the Browser?

The `Access-Control-Allow-Origin` header allows servers to specify how their resources are shared with external domains.

When a `GET` request is made to a server `domain-b.com`:

```
GET /data HTTP/1.1
Host: domain-b.com
...
Origin: https://domain-a.com
```

The response can include a header which means access is allowed from any domain.

```
HTTP/1.1 200 OK
...
Access-Control-Allow-Origin: *
```

Or it might restrict access to specific domains:

```
Access-Control-Allow-Origin: https://domain-a.com
```

It also can additionally respond with this header:

```
Access-Control-Allow-Credentials: true
```

Which means the cross-domain request can include cookies and so will be processed in-session..

<br>

### Pre-flight Requests

In order to make data manipulation requests the browser will first sends a `preflight` request with `options` header.

The server will respond to the preflight request and indicate whether or not the original request is safe. Otherwise, it will block the original request.

<br>

### Other HTTP headers

- Access-Control-Allow-Headers
- Access-Control-Allow-Methods
- Access-Control-Expose-Headers
- Access-Control-Max-Age
- Access-Control-Request-Headers
- Access-Control-Request-Method
- Origin

<br>

## How to prevent CORS attacks
- Only allow trusted sites
- Avoid whitelisting null (`Access-Control-Allow-Origin: null`)
- Avoid wildcards in internal networks

<br><br>

---
### References

- [What is CORS?](https://www.codecademy.com/articles/what-is-cors)
- [Cross-origin resource sharing (CORS)](https://portswigger.net/web-security/cors)
- [Same-origin policy (SOP)](https://portswigger.net/web-security/cors/same-origin-policy)
