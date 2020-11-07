# An overview of HTTP

In HTTP, clients and servers communicate by exchanging individual messages (as opposed to a stream of data).
 Client sends *`Request`* and server sends back a *`Response`*.

 HTTP is an application layer protocol that is sent over `TCP`, or over a `TLS-encrypted` TCP connection,

<br>

## Components of HTTP-based systems

Between the client and the server there are numerous entities, collectively called `proxies`.

They perform different operations and act as `gateways` or `caches`,

In reality, there are more computers between a browser and the server handling the request: there are `routers`, `modems`, and more. 

<br>

## Basic aspects of HTTP

* **HTTP is simple**: TTP messages can be read and understood by humans.
* **HTTP is extensible**: `HTTP headers` make this protocol easy to extend. New functionality can be introduced between a client and a server.
* **HTTP is stateless, but not sessionless**: there is no link between two requests being successively carried out on the same connection. But `HTTP cookies` allow the use of stateful sessions.
* **HTTP and connections**: HTTP relies on the `TCP` standard, which is connection-based. Before a client and server can exchange an HTTP request/response pair, they must establish a TCP connection, a process which requires several round-trips.

<br>

## HTTP flow

When a client wants to communicate with a server, these steps will happen:

1. Open a TCP connection: to send requests and receive responses.
2. Send an HTTP message
3. Read the response
4. Close or reuse the connection for further requests.

<br>