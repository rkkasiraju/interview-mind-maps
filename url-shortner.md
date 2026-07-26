                               Users
                                 │
                                 ▼
                         Global DNS / Anycast
                                 │
                                 ▼
                      Global Load Balancer (Traffic Manager)
                                 │
                                 ▼
                           Web Application Firewall
                                 │
                                 ▼
                            API Gateway / Ingress
                    (Authentication, Rate Limiting, Routing)
                                 │
             ┌───────────────────┴───────────────────┐
             ▼                                       ▼
     URL Shortener Service                  Analytics Service
   (Stateless Spring Boot Pods)          (Stateless Consumers)
             │                                       ▲
             │                                       │
             │                               Kafka Events
             │                                       ▲
             ▼                                       │
        Redis Cluster  ───────── Cache Aside ────────┘
             │
      Cache Miss
             ▼
     URL Mapping Database
      (Sharded + Replicated)
             │
      Primary → Read Replicas
             │
             ▼
        Backup / Snapshot

Observability:
Client → Gateway → Service → Redis → DB
        │
        ├── Prometheus Metrics
        ├── Grafana Dashboards
        ├── OpenTelemetry Traces
        └── ELK / Splunk Logs

Security:
HTTPS
OAuth/JWT (if authenticated APIs)
mTLS (service-to-service)
Secrets Manager
Certificate Manager
WAF
Rate Limiting
Safe Browsing Validation

Deployment:
GitHub Actions
→ Artifact Registry
→ Harness / ArgoCD
→ Kubernetes / OpenShift
→ Rolling / Blue-Green / Canary
