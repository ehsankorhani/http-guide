# HTTP caching

Caching is a technique that stores a copy of a given resource and serves it back when requested.

- **Shared cache** is a cache that stores responses for reuse by more than one user.
- **Private cache** is dedicated to a single user. 

HTTP caches are typically limited to caching responses to `GET`.

<br>

## Controlling caching

### `Cache-Control` header
Is used to specify directives for caching mechanisms in both requests and responses.

**No caching**: The cache should not store anything about the client request or server response.

```
Cache-Control: no-store
```

**Cache but revalidate
**:
A cache will send the request to the origin server for validation before releasing a cached copy.

```
Cache-Control: no-store
```

**Private and public caches**:
The `public` directive indicates that the response may be cached by any cache.<br>
`private` indicates that the response is intended for a single user only and must not be stored by a shared cache. \

```
Cache-Control: private
Cache-Control: public
```

**Expiration**:
The maximum amount of time (in seconds) in which a resource will be considered fresh.<br>
It is relative to the time of the request, and overrides the Expires header (if set).

```
Cache-Control: max-age=31536000
```

**Validation**:
Once the cache expires, refuse to return stale responses to the user even if they say that stale responses are acceptable.<br>
With `must-revalidate`, if the server doesn't respond to a revalidation request, the browser/proxy is supposed to return a 504 error.<br>
With `no-cache`, it would just show the cached content.

```
Cache-Control: must-revalidate
```

<br>

### `Pragma` header

`Pragma` should only be used for backwards compatibility with HTTP/1.0

```
Pragma: no-cache
```

<br>

## Freshness
Before this expiration time, the resource is *fresh*.<br>
After the expiration time, the resource is *stale*.

A stale resource is not evicted or ignored. when the cache receives a request for a stale resource, it forwards this request with a `If-None-Match` to check if it is in fact still fresh. If so, the server returns a 304 (Not Modified) header without sending the body of the requested resource.

![Http Structure](./img/http-staleness.png)

1. If a `Cache-Control: max-age=N` header is specified, then the freshness lifetime is equal to `N`.
2. If this header is not present, it is checked if an `Expires` header is present.
3. If an `Expires` header exists, then its value minus the value of the `Date` header determines the freshness lifetime.
4. If non of above exist then a *heuristic* approach may be used. The cache's freshness lifetime is equal to the value of the `Date` header minus the value of the `Last-modified` header divided by 10.

<br>

## Cache validation

When a cached document's expiration time has been reached, it is either validated or fetched again. 

Validation can happen with the following headers:

### ETags
Can be used as a strong validator. *user-agent* knows nothing about the meaning of the value.


If the `ETag` header was part of the response, the client can issue an `If-None-Match` in the header of future requests.


<br>

### Last-Modified

Is a weak validator.

If the `Last-Modified` header is present in a response, then the client can issue an `If-Modified-Since` request header to validate the cached document.

Server response with a normal `200 OK`, or it can return `304 Not Modified` (with an empty body) to instruct the browser to use its cached copy.
