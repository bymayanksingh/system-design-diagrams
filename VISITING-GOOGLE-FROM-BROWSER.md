```mermaid
sequenceDiagram
    participant B as Browser
    participant DNS as DNS Server
    participant LB as Load Balancer
    participant RL as Rate Limiter
    participant S as Google Server
    participant C as Cache
    participant DB as Database
    participant R as Renderer
    participant JS as JavaScript Engine
    participant Cookie as Cookie Manager

    B->>DNS: Requests IP for www.google.com
    DNS-->>B: Returns IP of Load Balancer
    B->>LB: Initiates TCP/IP handshake
    LB-->>B: Completes TCP/IP handshake
    B->>RL: Sends HTTP/HTTPS GET request
    RL->>B: Verifies request rate limit
    RL->>S: Forwards validated GET request
    S->>C: Checks Cache for data
    alt Cached data available
        C-->>S: Returns Cached data
    else Cached data not available
        S->>DB: Retrieves data from Database
        DB-->>S: Returns requested data
        S->>C: Stores data in Cache for future use
        C-->>S: Returns Cached data
    end
    S-->>B: Sends HTTP response (HTML, CSS, JS)
    B->>R: Parses HTML & CSS
    B->>JS: Executes JavaScript
    JS-->>B: Modifies DOM as necessary
    S->>Cookie: Sends "Set-Cookie" Header
    Cookie-->>B: Stores and manages Cookies
    B->>S: Sends any subsequent requests (AJAX, etc.) with Cookies
    S-->>B: Returns additional data
    B->>LB: Initiates TCP/IP teardown
    LB-->>B: Completes TCP/IP teardown
```

## What happens when you visit google.com from browser?

1. **DNS Lookup:** Same as before, the browser begins by translating "www.google.com" into an IP address using the DNS.

2. **Load Balancing:** The IP address obtained from DNS doesn't point directly to a single server but rather to a Load Balancer. Google's infrastructure includes many servers, and the load balancer distributes network or application traffic across these servers to ensure no single server becomes overwhelmed and to increase availability and reliability.

3. **Rate Limiting:** Load balancers or servers can also have a rate limiter in place to control the amount of incoming requests a user can send in a given amount of time. This is used to prevent abuse, DDoS attacks, and to ensure service availability. If a rate limiter was triggered, you'd receive an error message back at this point.

4. **TCP/IP Three-Way Handshake:** The same handshake process then happens between your browser and Google's load balancer or server.

5. **HTTP/HTTPS Request:** The client sends an HTTP GET request, with HTTPS used for secure communication.

6. **Server-Side Processing and Caching:** Google's servers receive the HTTP GET request. The requested data may be served from the cache if it's a commonly requested or static page, speeding up the process. Cache systems can be distributed and placed at various points in the system design, for instance, Content Delivery Network (CDN), Reverse Proxy or even at Database level (like Redis).

7. **HTTP/HTTPS Response:** The server responds to the GET request with the HTTP response. The response contains the website content and may contain a "Set-Cookie" HTTP header to manage user sessions.

8. **Web Rendering and JavaScript Execution:** After receiving the response, the client's browser parses, renders the page, and executes JavaScript as before.

9. **Cookies and Sessions:** HTTP Cookies set by the server are stored and managed by the client.

10. **TCP Connection Teardown:** After all resources are loaded and scripts are executed, the TCP connection is terminated.

This is a broader and more complex system design view that includes load balancing, rate limiting, and caching. However, there can still be even more layers of complexity when you get into the specifics of Google's architecture. It's a global, multi-tiered system that involves numerous data centers, networks, servers, and software components all working together to deliver high-speed, reliable, and secure web services.
