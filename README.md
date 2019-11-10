# Understanding & Measuring HTTP Timings with Node.js

Understanding and measuring HTTP timings helps us to discover performance bottlenecks in *client to server* or *server to server* communication. This will explains timings in an HTTP request and shows how to measure them using Node.js.

Before we jump into HTTP timings, let's take a look at some basic concepts:

- **IP** *(Internet Protocol):* IP is a network-layer protocol, deals with network addressing and routing. IP is responsible for delivering packets from the source host to the destination host based on the packet headers across one or more IP networks. It also defines packet structures that encapsulate the data to be delivered.

- **DNS** *(Domain Name Servers):* DNS is a hierarchical decentralized naming system used to resolve human-readable hostnames like github.com into machine-readable IP addresses.

- **TCP** *(Transmission Control Protocol):* The TCP standard defines how to establish and maintain a network conversation between applications to exchange data. TCP provides reliable, ordered, and error-checked delivery of a stream of octets between applications running on hosts communicating over an IP network. An HTTP client initiates a request by establishing a TCP connection.

- **SSL/TLS** *(Transport Layer Security):* TLS is a cryptographic protocol that provides communications security over a computer network. SSL (Secure Sockets Layer) is a deprecated predecessor to TLS. Both TLS and SSL use certificates to establish a secure connection. SSL certificates are not dependent on cryptographic protocols like TLS, a certificate contains a key pair: a public and a private key. These keys work together to establish an encrypted connection.

**Now let's take a look at the timeline of a usual HTTP Request:**

![alt text](./images/http-timing-nodejs.png)

Timings explained:

**DNS Lookup:** Time spent performing the DNS lookup. DNS lookup resolves domain names to IP addresses. Every new domain requires a full round trip to do the DNS lookup. There is no DNS lookup when the destination is already an IP address.

**TCP Connection:** Time it took to establish TCP connection between a source host and destination host. Connections must be properly established in a multi-step handshake process. TCP connection is managed by an operating system, if the underlying TCP connection cannot be established, the OS-wide TCP connection timeout will overrule the timeout config of our application.

**TLS handshake:** Time spent completing a TLS handshake. During the handshake process endpoints exchange authentication and keys to establish or resume secure sessions. There is no TLS handshake with a not HTTPS request.

**Time to First Byte (TTFB):** Time spent waiting for the initial response. This time captures the latency of a round trip to the server in addition to the time spent waiting for the server to process the request and deliver the response.

**Content Transfer:** Time spent receiving the response data. The size of the response data and the available network bandwidth determinates its duration.

**How do HTTP timings help to discover bottlenecks?**
For example, if your **DNS Lookup** takes longer time than you expected, the issue might be with your DNS provider or with your DNS caching settings.

When you see longer **Time to First Byte** durations, you should check out the latency between the endpoints, but you should also check out the current load of the server.

Slow **Content Transfer** can be caused by inefficient response body like sending back too much data (unused JSON properties, etc.) or by a slow connection as well.

## Measuring HTTP timings in Node.js

To measure HTTP timings in Node.js, we need to subscribe to a specific request, response and socket events. Here is a short code snippet how to do this in Node.js, this example focuses only to the timings:

## Reference

[Node.js Tutorials for Beginners](https://blog.risingstack.com/measuring-http-timings-node-js/)
