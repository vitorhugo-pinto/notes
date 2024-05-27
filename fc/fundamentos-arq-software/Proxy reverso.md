[reverse proxy by cloudflare](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)
A reverse proxy is a server that sits in front of web servers and forwards client (e.g. web browser) requests to those web servers. Reverse proxies are typically implemented to help increase security, performance, and reliability. In order to better understand how a reverse proxy works and the benefits it can provide, let’s first define what a proxy server is.

![[Screenshot 2024-05-17 at 04.21.30.png]]

The difference between a forward and reverse proxy is subtle but important. A simplified way to sum it up would be to say that a forward proxy sits in front of a client and ensures that no origin server ever communicates directly with that specific client. On the other hand, a reverse proxy sits in front of an origin server and ensures that no client ever communicates directly with that origin server.

![[Screenshot 2024-05-17 at 04.23.22.png]]

Below we outline some of the benefits of a reverse proxy:

- **[Load balancing](https://www.cloudflare.com/learning/cdn/cdn-load-balance-reliability/)** - A popular website that gets millions of users every day may not be able to handle all of its incoming site traffic with a single origin server. Instead, the site can be distributed among a pool of different servers, all handling requests for the same site. In this case, a reverse proxy can provide a load balancing solution which will distribute the incoming traffic evenly among the different servers to prevent any single server from becoming overloaded. In the event that a server fails completely, other servers can step up to handle the traffic.
- **Protection from attacks** - With a reverse proxy in place, a web site or service never needs to reveal the IP address of their origin server(s). This makes it much harder for attackers to leverage a targeted attack against them, such as a [DDoS attack](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/). Instead the attackers will only be able to target the reverse proxy, such as Cloudflare’s [CDN](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/), which will have tighter security and more resources to fend off a cyber attack.
- **[Global server load balancing](https://www.cloudflare.com/learning/cdn/glossary/global-server-load-balancing-gslb/) (GSLB)** - In this form of load balancing, a website can be distributed on several servers around the globe and the reverse proxy will send clients to the server that’s geographically closest to them. This decreases the distances that requests and responses need to travel, minimizing load times.
- **Caching** - A reverse proxy can also [cache](https://www.cloudflare.com/learning/cdn/what-is-caching/) content, resulting in faster performance. For example, if a user in Paris visits a reverse-proxied website with web servers in Los Angeles, the user might actually connect to a local reverse proxy server in Paris, which will then have to communicate with an origin server in L.A. The proxy server can then cache (or temporarily save) the response data. Subsequent Parisian users who browse the site will then get the locally cached version from the Parisian reverse proxy server, resulting in much faster performance.
- **SSL encryption** - [Encrypting](https://www.cloudflare.com/learning/ssl/what-is-encryption/) and decrypting [SSL](https://www.cloudflare.com/learning/ssl/what-is-ssl/) (or [TLS](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/)) communications for each client can be computationally expensive for an origin server. A reverse proxy can be configured to decrypt all incoming requests and encrypt all outgoing responses, freeing up valuable resources on the origin server.