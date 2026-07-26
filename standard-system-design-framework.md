# Standard System Design Framework

**For Principal/Lead Engineers: A Reusable Decision Model Across All Distributed Systems**

---

## Overview

This framework provides a systematic approach to analyzing and designing distributed systems. Rather than memorizing 21+ distinct systems, master **one decision framework** and **one component library**, then every system becomes a combination of reusable building blocks.

---

## Part 1: The Core Design Template

Every system design interview—and every real production system—can be systematically analyzed using this sequence:

### 1. Problem Statement
- **What business problem are we solving?**
- **Why does this system need to exist?**
- **Explicit scope boundaries** (in-scope and out-of-scope)
- **Success metrics** (business objectives)
- **Key stakeholders and use cases**

### 2. Functional Requirements
- **Core features** (essential capabilities)
- **Non-essential features** (explicit out-of-scope)
- **User workflows** and interactions
- **Data operations** required (CRUD + specific queries)
- **System boundaries** and external dependencies

### 3. Non-Functional Requirements
- **Scalability**: Capacity growth expectations (100x? 1000x?)
- **Availability**: Uptime SLA (99.9% = 8.76 hours downtime/year, 99.99% = 52 minutes)
- **Latency**: P50/P95/P99 response times and timeout budgets
- **Consistency**: Strong vs eventual consistency requirements
- **Durability**: Data loss tolerance and backup requirements
- **Security**: Authentication, authorization, encryption, compliance
- **Cost**: Infrastructure budget constraints and cost optimization priorities

### 4. Capacity Estimation
- **User base**: Daily Active Users (DAU), Monthly Active Users (MAU), concurrent users
- **Request rate**: Requests per second (RPS) at peak vs average
- **Storage requirements**: Initial, growth rate, retention policies
- **Bandwidth requirements**: Network I/O calculations
- **Growth assumptions**: Business projections and scaling factors
- **Regional distribution**: Geographical spread of users

### 5. High-Level Architecture
- **Major system components** and their responsibilities
- **Communication patterns** (Synchronous RPC vs Asynchronous messaging)
- **Data flow** (request path through system)
- **Deployment topology** (monolithic vs microservices)
- **Service boundaries** and ownership model
- **Technology stack** rationale

### 6. Data Model
- **Entities** and their attributes
- **Relationships** between entities (1:1, 1:N, N:N)
- **Database choice** justification (SQL vs NoSQL)
  - SQL: ACID guarantees, strong consistency, complex joins
  - NoSQL: Horizontal scaling, flexible schema, eventual consistency
- **Partition key** strategy (critical for horizontal scaling)
- **Indexes** strategy (read performance optimization)
- **Query patterns** expected

### 7. APIs
- **Protocol choice**: REST (HTTP), gRPC, GraphQL, protobuf
- **Request/Response schemas** with examples
- **Idempotency keys** for safety (critical for financial systems)
- **Pagination strategy** for large result sets
- **Versioning approach** (URL, header, or accept-encoding)
- **Error handling** and retry semantics

### 8. Write Path (Request Lifecycle)
- **Step-by-step request flow** from client to storage
- **Transactional boundaries** and consistency guarantees
- **Event emission** and downstream triggers
- **Cache invalidation** strategy
- **Acknowledgment model** (at-most-once vs at-least-once)
- **Duplicate detection** mechanisms

### 9. Read Path (Query Optimization)
- **Cache lookup** (L1: local, L2: distributed)
- **Database query** execution (including index strategy)
- **Response assembly** and transformation
- **Read optimization techniques** (denormalization, materialized views)
- **Staleness tolerance** and cache TTL

### 10. Scaling Strategies
- **Horizontal scaling** approach for stateless services
- **Load balancing** strategy (round-robin, consistent hash, least-connections)
- **Sharding** key selection and hot partition mitigation
- **Replication** factor and read replica strategy
- **Caching layers** (Redis, memcached, in-process)
- **CDN** for static content and edge caching
- **Queue scaling** for async workloads

### 11. Reliability Mechanisms
- **Replication** strategy (active-active vs active-passive)
- **Retry logic** with exponential backoff and jitter
- **Circuit breaker pattern** to prevent cascade failures
- **Timeouts** and deadline propagation
- **Idempotency** key tracking for safe retries
- **Disaster Recovery (DR)** procedures and RTO/RPO targets
- **Backup strategy** (frequency, retention, test procedures)
- **Multi-region failover** mechanics

### 12. Security Architecture
- **Authentication**: OAuth2, SAML, JWT, mTLS
- **Authorization**: RBAC, ABAC, service-to-service permissions
- **Encryption**: At-rest (AES-256), in-transit (TLS 1.3)
- **Secrets management**: Vault, HSM, rotation policies
- **mTLS**: Service-to-service certificate management
- **Rate limiting**: Per-user, per-IP, per-service quotas
- **Audit logging**: Who accessed what and when
- **Compliance**: GDPR, HIPAA, SOC2 requirements

### 13. Monitoring & Operations
- **Metrics**: Resource utilization, business metrics, SLO tracking
- **Logs**: Structured logging, log aggregation, retention policies
- **Traces**: Distributed tracing (OpenTelemetry, Jaeger)
- **Dashboards**: Real-time visibility into system health
- **Alerts**: Threshold-based and anomaly detection rules
- **SLO/SLI**: Service Level Objectives and Indicators definition
- **Health checks**: Liveness and readiness probes
- **On-call playbooks**: Runbooks for common incidents

### 14. Failure Scenarios & Mitigations
- **Database down**: Read replicas, failover, fallback to cache
- **Cache failure**: Graceful degradation, cache warming
- **Queue failure**: Dead letter queues (DLQ), manual replay
- **Service crash**: Health checks, auto-restart, circuit breaker
- **Network partition**: Eventual consistency, reconciliation
- **Region outage**: Multi-region replication, failover
- **Duplicate requests**: Idempotency keys, deduplication
- **Poison messages**: DLQ, alerting, manual inspection
- **Cascading failures**: Bulkheads, timeouts, circuit breakers

### 15. Trade-offs Analysis
- **CAP Theorem**: Choose 2 of Consistency, Availability, Partition tolerance
- **SQL vs NoSQL**: ACID guarantees vs horizontal scalability
- **Sync vs Async**: Latency vs throughput
- **Push vs Pull**: Freshness vs complexity
- **Cache vs DB**: Latency vs consistency
- **Cost vs Performance**: Infrastructure spending vs user experience
- **Consistency vs Availability**: Strong vs eventual consistency
- **Complexity vs Flexibility**: Implementation simplicity vs adaptability

### 16. Future Improvements & Evolution
- **Multi-region expansion**: Handling global users and regulatory requirements
- **AI/ML integration**: Personalization, recommendations, anomaly detection
- **Advanced search**: Full-text search, NLP capabilities
- **Analytics**: Event-driven analytics pipelines
- **Event sourcing**: Complete audit trail and temporal queries
- **CQRS**: Command Query Responsibility Segregation for complex domains
- **Observability improvements**: Better tracing, profiling, debugging

---

## Part 2: The Universal Building Blocks Component Library

Don't memorize systems. Memorize these 20 reusable components and when to use each:

```
                         Internet
                             │
                      DNS / Anycast
                             │
                    Global Load Balancer
                             │
                          WAF / CDN
                             │
                      API Gateway
                             │
                Authentication / Authorization
                             │
                  Stateless Services (N instances)
         ┌─────────────────┼────────────────┐
         │                 │                │
         ▼                 ▼                ▼
    Cache Layer        Message Bus      Search Engine
  (Redis/Memcached)  (Kafka/RabbitMQ) (Elasticsearch)
         │                 │
         │                 ▼
         │           Async Workers
         │
         ▼
    Data Layer
  (SQL/NoSQL/Time-series)
         │
    Read Replicas
         │
    Backup / Archive
         │
    Monitoring Layer
    (Metrics, Logs, Traces)
         │
    Security Layer
    (TLS, JWT, OAuth, Secrets, Rate Limiting, mTLS)
```

**Key Principle**: Every architecture is simply **choosing which boxes become critical** and how to connect them.

---

### Component Deep Dives

#### 1. Load Balancer
**Use when**: Multiple service instances exist, need traffic distribution

**Appears in**: URL Shortener, Uber, Netflix, WhatsApp, most systems

**Key decisions**:
- Algorithm: round-robin, least-connections, consistent hashing
- Sticky sessions: Required for stateful services
- Health checking: Frequency and thresholds
- Session affinity: Performance vs complexity

---

#### 2. API Gateway
**Use when**: Single entry point needed, authentication, routing, rate limiting

**Appears in**: Almost every backend system

**Responsibilities**:
- Authentication (OAuth2, API keys)
- Request routing to services
- Rate limiting and quotas
- Request/response transformation
- Timeout management
- CORS handling

---

#### 3. Cache Layer (Redis/Memcached)
**Use when**: Read-heavy workload, high latency database

**Appears in**: URL Shortener, Social Feed, Authentication, Feature Flags, Search

**Cache patterns**:
- Cache-aside: Application responsible for cache misses
- Write-through: Synchronous cache updates
- Write-behind: Asynchronous cache updates (complex, can lose data)

**Consistency challenges**:
- Cache invalidation strategy
- TTL vs event-driven invalidation
- Cache stampede mitigation
- Distributed cache coherency

---

#### 4. Message Queue / Event Streaming (Kafka/RabbitMQ)
**Use when**: Don't make users wait for slow operations

**Appears in**: Notifications, Payments, Order Processing, Logging, Analytics, Recommendations, Chat

**Message guarantees**:
- At-most-once: May lose messages
- At-least-once: May duplicate (requires idempotency)
- Exactly-once: Hardest to implement, highest overhead

**Key considerations**:
- Topic partitioning (affects message ordering)
- Consumer group coordination
- Offset management and replayability
- Dead Letter Queue (DLQ) for failures
- Backpressure handling

---

#### 5. Database
**SQL (Relational) - Use when**:
- Strong ACID guarantees required
- Complex joins across tables
- Schema is well-defined and stable
- Transactions essential

Examples: PostgreSQL, MySQL, Oracle, SQL Server

**NoSQL - Use when**:
- Flexible schema required
- Horizontal scaling is critical
- Document or graph structure fits data
- Can tolerate eventual consistency

**Sub-types**:
- Key-value: Redis, DynamoDB
- Document: MongoDB, Cosmos DB
- Graph: Neo4j
- Time-series: InfluxDB, Prometheus
- Search: Elasticsearch

**Critical decisions**:
- Partition/shard key (impacts scalability)
- Replication factor (trade-off availability vs write latency)
- Consistency model (strong vs eventual)
- Backup and recovery strategy

---

#### 6. Search Engine (Elasticsearch)
**Use when**: Full-text search, ranking, complex filtering

**Appears in**: Amazon, Google, Netflix, YouTube

**Key capabilities**:
- Full-text search with ranking
- Aggregations and analytics
- Real-time indexing
- Distributed scalability

**Challenges**:
- Maintaining index freshness
- Managing index size
- Query complexity and performance
- Relevance tuning

---

#### 7. Object Storage (S3, Azure Blob, GCS)
**Use when**: Large files (videos, images, documents)

**Appears in**: Google Drive, Netflix, YouTube

**Design patterns**:
- Chunked uploads for large files
- CDN integration for distribution
- Lifecycle policies for archival
- Access control and signed URLs

---

#### 8. CDN (Content Delivery Network)
**Use when**: Static files, videos, images, global distribution

**Appears in**: YouTube, Netflix, Facebook, Google Drive

**Edge cases**:
- Cache invalidation and purging
- Origin failover
- DDoS protection at edge
- Regional content localization

---

#### 9. WebSocket
**Use when**: Realtime bidirectional communication

**Appears in**: WhatsApp, Uber, Trading platforms, Multiplayer games

**Challenges**:
- Connection affinity (user sticks to server)
- Graceful reconnection
- Message ordering for realtime updates
- Presence tracking and notifications
- Horizontal scaling (sticky sessions or state store)

---

#### 10. Geospatial Index (Geohash, QuadTree, R-tree)
**Use when**: Nearby object search (within radius, nearby drivers, nearby restaurants)

**Appears in**: Uber, DoorDash, Ola, Maps

**Algorithms**:
- Geohash: Simple, approximate, good for grid-based queries
- QuadTree: Hierarchical spatial partitioning
- R-tree: Minimum bounding rectangle trees

**Implementation**:
- Database native (PostGIS, MongoDB geo-indexes)
- Specialized services
- Caching nearby results

---

#### 11. Distributed Lock (Mutex)
**Use when**: Only one process should update at a time (prevent race conditions)

**Appears in**: Inventory management, Payment systems, Scheduling

**Implementations**:
- Database row-level locking
- Redis (SET NX, Redlock algorithm)
- ZooKeeper
- Distributed consensus

**Challenges**:
- Lock timeout and recovery
- Deadlock prevention
- Performance impact on throughput

---

#### 12. Scheduler (Cron, Job Scheduler)
**Use when**: Periodic or delayed task execution

**Appears in**: Recommendations, Cleanup jobs, Billing, CI/CD

**Key concerns**:
- Exactly-once execution (prevent duplicates)
- Failure recovery and retries
- Priority queuing
- Rate limiting between jobs
- Distributed scheduling (multiple instances)

---

#### 13. API Rate Limiter
**Use when**: Protect service from overload, enforce quotas

**Algorithms**:
- Token bucket: Allows burst traffic
- Leaky bucket: Smooth traffic
- Fixed window: Simple but inaccurate
- Sliding window: More accurate

**Scope**:
- Per-user
- Per-IP
- Per-service
- Global

---

#### 14. Service Discovery
**Use when**: Dynamic services joining/leaving

**Approaches**:
- Client-side: Service registry (Consul, Eureka)
- Server-side: Load balancer queries registry
- DNS-based: Service names resolve to IPs

---

#### 15. Circuit Breaker
**Use when**: Prevent cascading failures from slow/down services

**States**:
- Closed: Normal operation
- Open: Stop calling service, fast-fail
- Half-open: Test if service recovered

**Thresholds**:
- Failure percentage
- Request count threshold
- Timeout duration before retry

---

#### 16. Bulkhead Pattern
**Use when**: Prevent one failing component from bringing down entire system

**Implementation**:
- Thread pool isolation
- Process-level isolation
- Container-level isolation
- Resource quotas

---

#### 17. Dead Letter Queue (DLQ)
**Use when**: Messages fail processing repeatedly

**Workflow**:
- Message processing fails
- Retry logic exhausted
- Message moved to DLQ
- Manual inspection and replay

---

#### 18. Event Sourcing
**Use when**: Need complete audit trail, temporal queries

**Key benefits**:
- Complete history of all state changes
- Ability to replay events
- Easier debugging and auditing

**Challenges**:
- Event version management
- Query complexity (need projections)
- Event versioning when schema changes

---

#### 19. CQRS (Command Query Responsibility Segregation)
**Use when**: Complex read and write models differ significantly

**Separation**:
- Command side: Handles writes, maintains data consistency
- Query side: Optimized for specific read patterns

**Challenges**:
- Eventual consistency between models
- Complexity in implementation
- Debugging multi-step operations

---

#### 20. Distributed Tracing
**Use when**: Understanding request flow through microservices

**Components**:
- Instrumentation: Inject trace IDs in requests
- Trace propagation: Pass trace context through calls
- Collection: Centralized trace storage
- Visualization: Timeline view of request

**Benefits**:
- Identify latency bottlenecks
- Understand service dependencies
- Debugging production issues

---

## Part 3: The Decision Matrix

Map problems to solutions. **This table is reusable across every interview**:

| Problem Category        | Specific Problem        | Think About            | Typical Solution          |
|-------------------------|-------------------------|------------------------|---------------------------|
| **Read Scale**          | Too many reads          | Read optimization      | Redis, CDN, Read Replicas |
| **Write Scale**         | Too many writes         | Write scaling          | Sharding, Kafka           |
| **Processing**          | Long-running tasks      | Async processing       | Kafka, Queue, Workers     |
| **Storage**             | Huge files              | Storage solution       | Object Storage (S3)       |
| **Search**              | Fast search             | Query optimization     | Elasticsearch             |
| **Location**            | Nearby objects          | Geospatial queries     | QuadTree, Geohash, GIS DB |
| **Communication**       | Realtime updates        | Persistent connections | WebSocket, gRPC streams   |
| **Ordering**            | Message/event ordering  | Event sequencing       | Partition Key, Kafka      |
| **Idempotency**         | Duplicate requests      | Safety                 | Idempotency Key, Dedup DB |
| **Concurrency**         | Concurrent updates      | Data consistency       | Locking, Optimistic Lock  |
| **Scale**               | Millions of users       | Horizontal scaling     | Stateless Services        |
| **Geography**           | Global users            | Low latency            | CDN, Multi-region, Edge   |
| **Availability**        | High availability       | Failure handling       | Replication, Failover     |
| **Protection**          | Security                | Protection layers      | OAuth, JWT, WAF, mTLS     |
| **Visibility**          | Monitoring              | Observability          | Metrics, Logs, Traces     |
| **Consistency**         | Data accuracy           | Transactional integrity| ACID, Sagas, Event Source |
| **Privacy**             | Data privacy            | Encryption/isolation   | TLS, Column encryption    |

---

## Part 4: System Mapping - 20 Core Systems

Notice that only about 15-20 core concepts cover all major systems:

| System               | Primary Concept     | Key Components                    | Scaling Challenge        |
|----------------------|---------------------|-----------------------------------|--------------------------|
| URL Shortener        | Key-value lookup    | Redis, SQL, Sharding              | Cache misses, hot keys   |
| Distributed Cache    | Cache design        | Consistent Hashing, Replication   | Thundering herd          |
| API Gateway          | Request routing     | Gateway, Auth, Rate Limiter       | Connection limits        |
| Notification Service | Async messaging     | Kafka, Workers, Retry, DLQ        | Guaranteed delivery      |
| Payment System       | Money movement      | SQL, Saga, Idempotency, Outbox    | Atomicity, consistency   |
| Order Management     | Business workflow   | Kafka, Event Sourcing, CQRS       | State machine complexity |
| Inventory System     | Stock accuracy      | Locking, Transactions, Cache      | Race conditions          |
| Ride Sharing (Uber)  | Real-time location  | WebSocket, GeoHash, Kafka         | Connection scalability   |
| Chat System          | Messaging           | WebSocket, Kafka, Redis           | Message ordering         |
| Social Feed          | Timeline generation | Fan-out, Cache, Ranking           | Thundering herd          |
| Search Engine        | Full-text search    | Crawler, Indexer, Elasticsearch   | Index freshness          |
| Video Streaming      | Media delivery      | CDN, Object Storage, Transcoder   | Bandwidth cost           |
| File Storage         | File management     | Chunking, Metadata DB, S3         | Large uploads            |
| Logging Platform     | Log ingestion       | Kafka, Storage, Search            | Volume and retention     |
| Metrics Platform     | Monitoring          | Time-series DB, Scraping          | Cardinality explosion    |
| Recommendation       | Personalization     | Kafka, Feature Store, ML Service  | Latency and accuracy     |
| AI Chat Platform     | LLM integration     | Gateway, RAG, Vector DB           | Token limits, latency    |
| Feature Flags        | Configuration      | Cache, Pub/Sub, Redis             | Consistency across users |
| Authentication       | Identity            | OAuth2, JWT, Redis, Secrets       | Session management       |
| CI/CD                | Build & delivery    | Scheduler, Workers, Artifact Store| Resource contention      |

---

## Part 5: Knowledge Graph - Mental Model

```
                            System Design

                                 │

         ┌───────────────────────┼─────────────────────────┐
         │                       │                         │
         ▼                       ▼                         ▼
     Compute                Data Layer             Communication

  Stateless Services     SQL / NoSQL DB        REST API
  Containers             Redis Cache           gRPC
  Kubernetes             Search Engine         Kafka / MQ
  Auto Scaling           Object Storage        WebSocket
  Load Balancer          Time-series DB        Server Push

         │                       │                         │
         └──────────────┬────────┴──────────────┐
                        ▼                       ▼
                 Scalability              Reliability

                Horizontal Scaling    Retry + Backoff
                Sharding              Circuit Breaker
                Replication           Failover / HA
                Read Replicas         Backup + DR
                CDN / Cache           Bulkheads
                Load Balancing        DLQ + Deadletter

                        │
                        ▼
                   Security

        OAuth2 / OIDC
        JWT / Sessions
        mTLS
        WAF
        Secrets Management
        Rate Limiting
        Audit Logging

                        │
                        ▼
                  Observability

        Metrics (Prometheus, Datadog)
        Logs (ELK Stack, Splunk)
        Tracing (Jaeger, DataDog APM)
        Alerts (PagerDuty, OpsGenie)
        Dashboards (Grafana, Azure Monitor)
        SLO/SLI Tracking
```

---

## Part 6: Progressive Learning Roadmap

### Phase 1 — Foundation Systems
**Goal**: Understand core patterns

- **URL Shortener**: Request routing, caching, databases, sharding
- **API Gateway**: Authentication, routing, rate limiting
- **Distributed Cache**: Cache design, consistency, eviction policies

**Concepts mastered**: HTTP, databases, caching, hashing, basic scaling

---

### Phase 2 — Event-Driven Architecture
**Goal**: Handle async workflows and consistency

- **Notification Service**: Message queues, async workers, retries
- **Order Management**: Event sourcing, sagas, consistency
- **Payment System**: Transactions, idempotency, auditing
- **Inventory System**: Locking, race conditions, eventual consistency

**Concepts mastered**: Kafka, ACID, event sourcing, saga pattern, idempotency

---

### Phase 3 — Real-Time Systems
**Goal**: Handle persistent connections and location services

- **WhatsApp/Chat**: WebSockets, message ordering, presence
- **Uber/Ride Sharing**: Geospatial indexing, real-time updates, dispatch

**Concepts mastered**: WebSockets, geohashing, location services, ordering guarantees

---

### Phase 4 — Content Platforms
**Goal**: Handle massive scale and complex queries

- **Social Feed**: Fan-out, ranking algorithms, cache strategies
- **Search Engine**: Crawling, indexing, ranking
- **YouTube/Video Streaming**: CDN, transcoding, object storage
- **Google Drive/File Storage**: Chunking, metadata, versioning

**Concepts mastered**: Ranking, indexing, CDN, object storage, versioning

---

### Phase 5 — Platform Engineering
**Goal**: Build infrastructure for other systems

- **Authentication Platform**: OAuth2, JWT, session management
- **Feature Flags**: Configuration management, gradual rollouts
- **Logging Platform**: Log aggregation, retention, search
- **Metrics Platform**: Time-series databases, aggregation
- **CI/CD Platform**: Job scheduling, artifact management
- **Kubernetes Scheduler**: Resource management, scheduling

**Concepts mastered**: IAM, observability, infrastructure-as-code, container orchestration

---

### Phase 6 — AI/ML Systems
**Goal**: Integrate ML with distributed systems

- **AI Chat Platform**: LLM gateways, RAG, vector stores, token management
- **Recommendation System**: Feature stores, model serving, batch + real-time

**Concepts mastered**: Vector databases, feature stores, model serving, RAG patterns

---

## Part 7: Mindset Shift for Senior Engineers

By end of preparation, **stop thinking about system names**. Instead, think about **architectural characteristics**:

### Wrong Thinking ❌
> "This is a URL Shortener problem."

### Correct Thinking ✓
> "This is a **read-heavy**, **low-latency**, **key-value lookup** system with **high availability** requirements. Let me apply read optimization techniques: caching with consistent hashing, multiple read replicas, possibly sharding if write volume becomes bottleneck."

---

### Example Frameworks

**Read-Heavy System**:
> "This is **read-dominant** with **high QPS** requirements. Design approach: multi-layer caching (in-process → distributed cache → database), read replicas, possibly CDN, batch pre-warming."

**Write-Heavy System**:
> "This is **write-dominant** with **consistency** requirements. Design approach: sharding on write key, local write buffering, async batch processing, saga pattern for distributed transactions."

**Real-Time System**:
> "This requires **sub-second latency** with **ordered updates**. Design approach: WebSocket for persistent connections, Kafka for ordered message delivery by partition, local state snapshots."

**Financial System**:
> "This requires **strong consistency** and **auditability**. Design approach: ACID transactions, idempotency keys for all operations, event sourcing for audit trail, saga pattern for distributed workflows."

**Global System**:
> "This requires **low latency** across **multiple regions** for geographically distributed users. Design approach: multi-region replication, CDN for static content, edge computing, data locality strategies."

---

## Part 8: Interview Execution Framework

### For Every System Design Question

**5 minutes**: Ask clarifying questions
- Scale: DAU, QPS, storage?
- Scope: Geographic regions? Languages?
- Consistency: Real-time or eventual?
- SLOs: Availability target? Latency SLA?

**5 minutes**: High-level architecture
- Draw major components
- Explain communication patterns
- Identify the bottleneck

**10 minutes**: Deep dive into bottleneck
- If reads: Caching strategy, read replicas
- If writes: Sharding strategy, partitioning
- If latency: Multi-region, edge caching
- If consistency: Transactions, event sourcing

**5 minutes**: Discuss trade-offs and improvements
- Why SQL vs NoSQL?
- Why sync vs async?
- What would you do differently with 10x scale?

---

## Part 9: Key Principles for Enterprise Systems (Microsoft Scale)

### 1. **Assume Failure is Normal**
- Design for multi-region failover
- Implement circuit breakers and bulkheads
- Always have fallback strategies
- Test disaster recovery regularly

### 2. **Observability is Non-Negotiable**
- Structured logging from day one
- Distributed tracing on all requests
- Real-time metrics and alerting
- SLO tracking and error budgets

### 3. **Security is Layered**
- Defense-in-depth approach
- Secrets management and rotation
- mTLS between services
- Rate limiting and DDoS protection
- Audit logging for compliance

### 4. **Consistency Models Matter**
- Strong consistency: Critical for financial systems
- Eventual consistency: Acceptable for social feeds, analytics
- Choose explicitly, understand trade-offs
- Verify using formal verification tools when possible

### 5. **Organizational Alignment**
- Service boundaries should match team boundaries
- Clear ownership and escalation paths
- Well-defined SLOs and error budgets
- Runbooks and on-call procedures

### 6. **Simplicity Over Complexity**
- Use proven patterns, avoid premature optimization
- Avoid distributed consensus unless necessary
- Favor choreography over orchestration when possible
- Invest in tooling and automation

---

## Part 10: Common Interview Mistakes to Avoid

| Mistake                          | Impact              | Fix                                   |
|----------------------------------|---------------------|---------------------------------------|
| Jump to code too fast            | Miss requirements   | Spend 5 min on problem statement     |
| Ignore non-functional requirements | Impossible design  | Explicitly state SLOs upfront        |
| Over-architect for scale         | Unnecessary complexity | Start simple, scale bottleneck      |
| Forget about operations          | Can't run in production | Include monitoring and runbooks     |
| Ignore consistency requirements  | Data corruption     | Discuss consistency model explicitly |
| No fallback strategies           | Cascading failures  | Design for graceful degradation     |
| Forget idempotency               | Duplicate transactions | Plan idempotency keys upfront       |
| No disaster recovery plan        | Major incidents     | Discuss RTO/RPO and failover         |

---

## Summary

Master these 3 pillars:

1. **The Template** (16 sections): One framework for all systems
2. **The Components** (20 building blocks): Reusable pieces with clear use cases
3. **The Patterns** (Problem → Solution matrix): Map problems to solutions

With these, you can design any system by:
- Understanding the problem → choosing components → assembling architecture → discussing trade-offs

This approach has scaled from startups to companies handling billions of users.

---
Design a monitoring system
Design Google Docs
Design a ticket reservation system.
Design a system that solves a problem in your domain, but not at web scale.
Design the Facebook post privacy functionality.
Design a translation system for Meta apps. 
Design an auction system like eBay.
Design a logger system for a pool of apps.
How would you design a service like Instagram? Both from a product perspective, and from a system design perspective.
*Last Updated: 2024 | Target Audience: Principal/Lead Engineers at Microsoft and similar enterprises*
