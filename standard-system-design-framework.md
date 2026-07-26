### Standard System Design Framework

1. Problem Statement

   * What problem are we solving?
   * Why does this system exist?
   * Scope

2. Functional Requirements

   * Core features
   * Out of scope

3. Non-Functional Requirements

   * Scalability
   * Availability
   * Latency
   * Consistency
   * Durability
   * Security
   * Cost

4. Capacity Estimation

   * Users
   * Requests/sec
   * Storage
   * Bandwidth
   * Growth assumptions

5. High-Level Architecture

   * Major components
   * Communication (Sync vs Async)
   * Data flow
   * Deployment overview

6. Data Model

   * Entities
   * Relationships
   * Database choice
   * Partition key
   * Indexes

7. APIs

   * REST/gRPC
   * Request & Response
   * Idempotency
   * Pagination
   * Versioning

8. Write Flow

   * Step-by-step request lifecycle
   * Transactions
   * Events
   * Cache updates

9. Read Flow

   * Cache lookup
   * Database
   * Response path
   * Optimizations

10. Scaling

* Horizontal scaling
* Load balancing
* Sharding
* Replication
* Caching
* CDN (if applicable)
* Queue scaling

11. Reliability

* Replication
* Retry
* Circuit breaker
* Timeouts
* Idempotency
* Disaster Recovery
* Backup
* Multi-region

12. Security

* Authentication
* Authorization
* Encryption
* Secrets management
* mTLS
* Rate limiting
* Audit logging

13. Monitoring & Operations

* Metrics
* Logs
* Traces
* Dashboards
* Alerts
* SLO/SLI
* Health checks

14. Failure Scenarios

* Database down
* Cache failure
* Queue failure
* Service crash
* Network partition
* Region outage
* Duplicate requests
* Poison messages

15. Trade-offs

* CAP theorem
* SQL vs NoSQL
* Sync vs Async
* Push vs Pull
* Cache vs DB
* Cost vs Performance
* Consistency vs Availability

16. Future Improvements

* Multi-region
* AI integration
* Search
* Analytics
* Event sourcing
* CQRS
* Observability improvements

---
