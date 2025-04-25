# HTTP-evolution-in-recent-past
### HTTP Protocol Evolution: 0.9 to 3.0

---

## HTTP/0.9

### Overview

HTTP/0.9 was the original, one-line protocol that supported only the GET method and delivered a raw HTML response without headers or status codes ([MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Evolution_of_HTTP)). The request format consisted solely of `GET /resource` and the server replied with the contents of the resource as a continuous byte stream.

### Limitations and Scenarios

- **Lack of Metadata:** No headers meant no way to specify content type, length, or encoding, making it impossible to serve non-HTML resources or perform content negotiation.
- **No Status Codes:** Errors could only be conveyed by returning an HTML error page, complicating automated error handling.
- **Connection Overhead:** Each request required a new TCP connection, leading to high latency for pages with multiple assets (e.g., images).

<img width="619" alt="image" src="https://github.com/user-attachments/assets/4282dd7c-b86d-4c4a-b9e6-c21a7129344a" />


### Resolution in HTTP/1.1

HTTP/1.1 introduced headers, status codes, and persistent connections to address these limitations.

---

## HTTP/1.1

### Overview

Standardized in RFC 7230–7235, HTTP/1.1 became the dominant web protocol by 1999. It required the `Host` header to enable virtual hosting and supported methods beyond GET, such as POST, PUT, and DELETE.

### Problems Solved Compared to HTTP/0.9

- **Persistent Connections:** Default support for keep-alive connections allowed multiple requests and responses over a single TCP connection, reducing the cost of TCP handshake and teardown for pages with many resources.
- **Chunked Transfer Encoding:** Servers could send dynamically generated content without knowing the total size in advance by breaking the response into chunks.
- **Caching and Conditional Requests:** Introduction of headers like `If-Modified-Since` and `ETag` enabled more efficient caching strategies and bandwidth savings.

<img width="631" alt="image" src="https://github.com/user-attachments/assets/62412cf8-34c0-417b-9500-8664675e1c8a" />

### Remaining Challenges

- **Head-of-Line Blocking:** Despite persistent connections, HTTP/1.1 pipelining suffered from head-of-line blocking, where a slow response could delay subsequent requests.
- **Limited Server Push:** No standardized way for servers to proactively send resources before the client requested them.

---

## HTTP/2

### Overview

HTTP/2, published as RFC 7540 in May 2015, was designed to improve web performance while remaining fully compatible with HTTP/1.1 semantics. It introduced a binary framing layer, multiplexed streams, header compression (HPACK), and server push.

### Problems Addressed

- **Multiplexing:** Multiple requests and responses could be interleaved over a single TCP connection without waiting for earlier ones to complete, eliminating transaction-level head-of-line blocking.
- **Header Compression:** HPACK reduced header overhead by compressing repeat header fields and using indexing for previously seen values.
- **Server Push:** Servers could send additional resources to clients preemptively, anticipating future requests, which could improve page load times.

<img width="553" alt="image" src="https://github.com/user-attachments/assets/d614bc13-af0d-46a3-8691-cf57a15676ef" />


### New Challenges

- **TCP Head-of-Line Blocking:** Although HTTP/2 fixed application-layer blocking, packet loss at the TCP level still stalled all multiplexed streams on that connection.
- **Complex Frame Management:** Binary framing and stream prioritization added implementation complexity for both clients and servers.

---

## HTTP/3

### Overview

HTTP/3, specified in RFC 9114 (June 2022), remaps HTTP semantics onto the QUIC transport protocol, which runs over UDP. QUIC integrates TLS 1.3, provides stream multiplexing, per-stream flow control, and 0-RTT connection establishment to reduce latency and eliminate TCP-level head-of-line blocking.

### Problems Addressed

- **Elimination of TCP Head-of-Line Blocking:** QUIC’s independent streams ensure that packet loss only stalls the affected stream, not the entire connection.
- **Faster Connection Setup:** Integrated TLS handshake and transport negotiation enable fewer round trips, often allowing data to flow in 0-RTT configurations.
- **Connection Migration:** QUIC supports seamless client IP changes (e.g., mobile roaming) without tearing down and re-establishing the connection.

<img width="613" alt="image" src="https://github.com/user-attachments/assets/43797847-1f3d-4dca-8a84-091e3f7a4e65" />


### Remaining Considerations

- **Middlebox Compatibility:** UDP-based QUIC may be blocked or throttled by firewalls and network middleboxes expecting TCP traffic.
- **Implementation Maturity:** While major browsers and CDNs support HTTP/3, some server and infrastructure components are still catching up.

---

## Useful Links

- [RFC 1945 – HTTP/1.0](https://datatracker.ietf.org/doc/html/rfc1945)
- [RFC 7230–7235 – HTTP/1.1](https://datatracker.ietf.org/doc/html/rfc7230)
- [RFC 7540 – HTTP/2](https://datatracker.ietf.org/doc/html/rfc7540)
- [RFC 9114 – HTTP/3](https://datatracker.ietf.org/doc/html/rfc9114)
- [MDN Web Docs: Evolution of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Evolution_of_HTTP)
- [HTTP/2 on Wikipedia](https://en.wikipedia.org/wiki/HTTP/2)
