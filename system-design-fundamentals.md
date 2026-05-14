# System Design Fundamentals

Quick reference for the building blocks you'll encounter when designing systems.

## Load Balancers

Distributes incoming traffic across multiple servers to increase capacity and reliability.

**Types:**
- **Layer 4 (Transport)** — routes by IP and port
- **Layer 7 (Application)** — routes by HTTP headers, cookies, or URL path

**Common Algorithms:**
- **Round Robin** — requests go to servers in sequence
- **Least Connections** — sends to the server with fewest active connections
- **IP Hash** — same client IP always hits the same server (session affinity)

## Web Server

Processes HTTP requests and serves responses. Sits between the client and your application.

Examples: Nginx, Apache

## Cache

Stores data so future requests are served faster.

**Strategies:**
- **Cache-Aside** — app reads/writes the cache itself
- **Read-Through** — cache reads from DB on behalf of the app
- **Write-Through** — data written to cache and DB simultaneously
- **Write-Back** — data written to cache first, then DB asynchronously

**Common tools:** Redis, Memcached

## Application Server

Hosts your application logic and business rules. Often sits behind a web server or load balancer.

## Database Server

Stores and serves data.

**Types:**
- **Relational (SQL)** — tables with rows and columns. Examples: PostgreSQL, MySQL
- **NoSQL** — document, key-value, column-family, or graph. Examples: MongoDB, DynamoDB, Cassandra

**When to use which:**
| Scenario | Pick |
|---|---|
| Structured data, complex queries, transactions | SQL |
| Flexible schema, high write throughput, horizontal scaling | NoSQL |
| Need both | Start with SQL, add NoSQL for specific workloads |

## Message Queues & Streaming

Enables async communication between services.

- **Message Queue** — point-to-point (producer → consumer). Examples: RabbitMQ, Amazon SQS
- **Pub/Sub** — one message goes to all subscribers. Examples: Google Cloud Pub/Sub
- **Streaming** — real-time processing of continuous data. Examples: Apache Kafka, Amazon Kinesis

**When to use:** When a service doesn't need an immediate response (e.g., sending emails, processing orders, analytics).

## CDN (Content Delivery Network)

Distributes content from servers geographically close to users. Reduces latency for static assets (images, JS, CSS).

Examples: Cloudflare, AWS CloudFront

## API Styles

- **REST** — stateless, uses HTTP methods (GET, POST, PUT, DELETE) on resources
- **GraphQL** — client specifies exactly what data it needs
- **gRPC/RPC** — call remote functions as if they were local; uses Protocol Buffers for efficiency

## API Gateway

Single entry point for all APIs. Handles:
- Authentication & authorization
- Rate limiting
- Request routing
- Response transformation

## Observability

Understanding what's happening inside your system from the outside.

**Three Pillars:**
- **Logs** — record of events (what happened)
- **Metrics** — numeric measurements over time (how much/how fast)
- **Traces** — follow a request end-to-end across services (where it went)

## Full-Text Search

Search engines optimized for searching text content across large datasets.

Examples: Elasticsearch, Apache Solr

**When to use:** When users need to search by keywords, partial matches, or fuzzy matching.

## Cloud Service Models

- **IaaS** — rent virtual machines and storage (e.g., AWS EC2)
- **PaaS** — deploy code without managing servers (e.g., Heroku, AWS Elastic Beanstalk)
- **SaaS** — use software via subscription (e.g., Gmail, Notion)

## Quick Design Checklist

Before building, ask yourself:
1. What are the core entities and relationships?
2. What's the expected read vs write ratio?
3. Do I need real-time or is eventual consistency OK?
4. Where are my users geographically?
5. What's my expected scale (users, requests/sec, data size)?
6. Which parts need caching?
7. Which parts can be async?

---

Adapted from [java-tech-interviews-prep](https://github.com/marttp/java-tech-interviews-prep)
