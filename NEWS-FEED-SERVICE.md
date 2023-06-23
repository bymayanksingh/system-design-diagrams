```mermaid
sequenceDiagram
    participant User1 as User 1
    participant User2 as User 2
    participant LB as Load Balancer
    participant WS as Web Server
    participant AS as Application Server
    participant DB as Database
    participant RS as Ranking Service
    participant C as Cache
    participant DS as Dedicated Service
    participant ERS as External Ranking Service

    User1->>LB: Request for news feed
    LB->>WS: Forward request for news feed
    WS->>AS: Request for news feed
    AS->>DB: Retrieve User 1 follows
    DB-->>AS: User 1 follows data
    AS->>DB: Retrieve posts of followed entities
    DB-->>AS: Posts data
    AS->>RS: Rank posts
    RS-->>AS: Ranked posts
    AS->>C: Store ranked posts in cache
    C-->>AS: Acknowledge storage
    AS-->>WS: Return top 20 posts
    WS-->>LB: Forward top 20 posts
    LB-->>User1: Display news feed

    Note over User1,LB: User scrolls to end of feed
    User1->>LB: Request for next 20 posts
    LB->>WS: Forward request for next 20 posts
    WS->>AS: Request for next 20 posts
    AS->>C: Fetch next 20 posts from cache
    C-->>AS: Next 20 posts
    AS-->>WS: Return next 20 posts
    WS-->>LB: Forward next 20 posts
    LB-->>User1: Display next 20 posts

    Note over User2,LB: User creates a new post
    User2->>LB: New post
    LB->>WS: Forward new post
    WS->>AS: Process new post
    AS->>DB: Store new post
    DB-->>AS: Acknowledge storage
    AS->>DS: Retrieve followers of User 2
    DS-->>AS: Followers data
    AS->>C: Update followers' cached feeds
    C-->>AS: Acknowledge update
    AS->>ERS: Request external service to notify followers of new post
    ERS-->>AS: Acknowledge notification
    AS-->>WS: Confirm post submission
    WS-->>LB: Confirm post submission
    LB-->>User2: Confirm post submission

    alt Unsuccessful Post Submission
        AS-->>WS: Report error
        WS-->>LB: Report error
        LB-->>User2: Report error
    end
```

- Note
A web server accepts and fulfills requests from clients for static content (i.e., HTML pages, files, images, and videos) from a website. Web servers handle HTTP requests and responses only.

An application server exposes business logic to the clients, which generates dynamic content. It is a software framework that transforms data to provide the specialized functionality offered by a business, service, or application. Application servers enhance the interactive parts of a website that can appear differently depending on the context of the request.

The illustration below highlights the difference in their architecture:

svg viewer

The columns below summarize the key differences between the two types of servers:

Web Server
Deliver static content.
Content is delivered using the HTTP protocol only.

Serves only web-based applications.
No support for multi-threading.

Facilitates web traffic that is not very resource intensive.
Application Server
Delivers dynamic content.
Provides business logic to application programs using several protocols (including HTTP).
Can serve web and enterprise-based applications.
Uses multi-threading to support multiple requests in parallel.
Facilitates longer running processes that are very resource-intensive​.
