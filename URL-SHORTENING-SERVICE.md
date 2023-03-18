```mermaid
sequenceDiagram
  participant User
  participant API Gateway
  participant Application Server
  participant Cache
  participant Load Balancer
  participant Web Server
  participant Database
  participant Monitoring

  User->>+API Gateway: Request to shorten URL
  API Gateway->>+Load Balancer: Forward request
  Load Balancer->>+Web Server: Forward request
  Web Server->>+Application Server: Forward request
  Application Server->>+Cache: Check if URL is cached
  Cache-->>-Application Server: Cached URL or Not Found
  alt URL is cached
    Application Server->>-Web Server: Return cached URL
    Web Server-->>-Load Balancer: Cached URL
    Load Balancer-->>-API Gateway: Cached URL
    API Gateway-->>-User: Cached URL
  else URL is not cached
    Application Server->>+Database: Query database for original URL
    Database-->>-Application Server: Original URL or Not Found
    alt URL is found in database
      Application Server->>+Cache: Store URL in cache
      Cache-->>-Application Server: Cached URL
      Application Server->>-Web Server: Return shortened URL
      Web Server-->>-Load Balancer: Shortened URL
      Load Balancer-->>-API Gateway: Shortened URL
      API Gateway-->>-User: Shortened URL
      Application Server->>+Monitoring: Log URL creation event
    else URL is not found in database
      Application Server->>-Web Server: Return error message
      Web Server-->>-Load Balancer: Error message
      Load Balancer-->>-API Gateway: Error message
      API Gateway-->>-User: Error message
      Application Server->>+Monitoring: Log error event
  end
```
