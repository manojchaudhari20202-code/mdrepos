# Microservices

###  Microservices & Service-Oriented Architecture Fundamentals
- **Microservices vs SOA concepts**: SOA = enterprise‑wide service reuse, often with ESB; microservices = fine‑grained, decentralized, independently deployable services.
- **Evolution of distributed architectures**: Monolith → SOA → microservices → serverless; driven by scalability, agility, cloud‑native.
- **Service boundaries**: Defined by business capabilities / subdomains; aligned with DDD bounded contexts.
- **Bounded contexts**: Core DDD concept; each service owns a specific domain model with clear boundaries and ubiquitous language.
- **Domain‑driven service design**: Services model business domains, not technical layers.
- **Stateless services**: No client session state stored; externalize to databases/caches for horizontal scaling.
- **Service autonomy**: Each service can be developed, deployed, and scaled independently.
- **Service granularity**: Balance between too coarse (monolithic) and too fine (overhead); based on business functions and team ownership.
- **API contracts**: Well‑defined interfaces (REST, gRPC, GraphQL) with versioning and documentation.
- **Loose coupling**: Minimal dependencies; changes in one service don’t force changes in others.
- **High cohesion**: Related functionality stays together inside a service.
- **Service ownership**: Teams own full lifecycle – code, data, operations.
- **Independent deployment**: Services deployable without affecting others; requires backward compatibility.
- **Service lifecycle**: Design → develop → deploy → maintain → decommission; includes versioning and deprecation.
- **Service discovery**: Mechanism (client‑side / server‑side) to find network locations of services; uses service registry.
- **API gateway fundamentals**: Single entry point for clients; handles routing, auth, rate limiting, aggregation.
- **Inter‑service communication**: Methods like HTTP/REST, gRPC, messaging; choice based on sync/async needs.
- **Synchronous vs asynchronous calls**: Sync = request‑response (blocking); async = queues/events (non‑blocking, decoupled).
- **REST vs messaging**: REST for request‑driven CRUD; messaging for event‑driven, reliable, decoupled communication.
- **Event‑driven services**: Communicate via events; publish‑subscribe; enable eventual consistency.
- **Service governance**: Policies for API standards, security, monitoring, lifecycle management.
- **Versioning strategies**: URI versioning, header versioning, semantic versioning; aim for backward compatibility.
- **Backward compatibility**: New service versions must support old clients; avoid breaking changes.
- **Contract‑first design**: Define API contracts before implementation; ensures consistency and parallel development.

###  Advanced Microservices & SOA
- **Distributed system design**: Design for failure, network latency, partial failures; use retries, circuit breakers.
- **Data consistency models**: Strong vs eventual consistency; choose based on business requirements.
- **CAP theorem in microservices**: Trade‑off between Consistency, Availability, Partition tolerance; microservices often choose AP or CP per use case.
- **Saga orchestration**: Central coordinator manages distributed transaction steps and compensating actions.
- **Choreography vs orchestration**: Choreography uses events for decentralized coordination; orchestration uses a central controller.
- **Event sourcing**: Store state changes as events; rebuild state by replaying events; enables auditability and temporal queries.
- **CQRS in microservices**: Separate command (write) and query (read) models; optimize for scalability and consistency.
- **Distributed transactions**: Avoid two‑phase commit; use sagas for eventual consistency.
- **Idempotency design**: Operations can be repeated without side effects; use idempotency keys.
- **Exactly‑once processing**: Achieve via idempotent consumers and deduplication; challenging in distributed systems.
- **Service mesh architecture**: Infrastructure layer for service‑to‑service communication; provides observability, security, traffic management.
- **Sidecar model**: Proxy container deployed alongside service; handles cross‑cutting concerns (routing, telemetry).
- **Multi‑region microservices**: Deploy across geographic regions for low latency and DR; consider data replication and consistency.
- **Hybrid deployments**: Mix of on‑prem and cloud; requires connectivity and consistent management.
- **Multi‑cloud architecture**: Use multiple cloud providers to avoid lock‑in, but increases complexity.
- **API federation**: Combine APIs from multiple services into a unified API for clients.
- **Cross‑service workflows**: Complex business processes spanning services; orchestrated via sagas or workflows.
- **Zero‑downtime deployments**: Deploy new versions without interruption; use rolling updates, blue‑green.
- **Blue‑green deployments**: Two environments (blue, green); switch traffic after validation.
- **Canary releases**: Gradually roll out new version to a subset of users.
- **Feature flags in microservices**: Toggle features on/off without deployment; enable gradual rollout and A/B testing.

###  Performance in Microservices & SOA
- **Latency in distributed systems**: Network calls add latency; minimize chattiness, optimize serialization.
- **Network overhead optimization**: Use efficient protocols (gRPC, HTTP/2), compression, persistent connections.
- **Service‑to‑service performance**: Monitor and optimize response times; use caching where appropriate.
- **Load balancing strategies**: Round‑robin, least connections, consistent hashing; client‑side vs server‑side.
- **Connection pooling**: Reuse connections to reduce overhead; configure pool sizes appropriately.
- **Caching strategies**: Local caches (in‑memory) vs distributed caches (Redis); cache‑aside, write‑through.
- **Edge caching**: Cache responses at CDN or API gateway to reduce latency and load.
- **Data locality**: Move compute close to data; use data partitioning and replication.
- **Bulkheads**: Isolate resources (thread pools, connections) to prevent one service from overwhelming others.
- **Rate limiting**: Control request rates to protect services from overload; per client or global.
- **Throttling**: Gradually reduce service quality under load (drop requests, degrade responses).
- **Backpressure**: Propagate load information upstream to slow down senders; prevents system overload.
- **Resource isolation**: Use containers, cgroups, or separate instances to isolate resource usage.
- **Autoscaling strategies**: Scale based on CPU, memory, request rate; horizontal scaling preferred.
- **Hotspot mitigation**: Detect and handle uneven load; use sharding, replication.
- **Throughput optimization**: Maximize requests per second; tune thread pools, async processing, DB queries.
- **Tail latency reduction**: Focus on high percentiles (p99); use hedging requests, request collapsing.

###  Instrumentation in Microservices & SOA
- **Runtime instrumentation**: Embed agents/libraries to collect metrics, traces, logs at runtime.
- **Service instrumentation libraries**: SDKs for metrics (Prometheus), tracing (OpenTelemetry), logging (structured loggers).
- **Metrics collection**: Gather quantitative data (request rates, error rates, latency) from services.
- **Distributed tracing instrumentation**: Instrument code to propagate trace context and record spans.
- **Logging instrumentation**: Generate structured logs with context (correlation IDs) for debugging.
- **Health probes**: Endpoints (liveness, readiness) for orchestration to check service health.
- **Telemetry pipelines**: Collect, aggregate, forward telemetry data to analysis tools (Fluentd, Logstash).
- **Profiling services**: Use continuous profiling to identify performance bottlenecks.
- **Failure injection**: Deliberately inject faults to test resilience (Chaos Mesh, Gremlin).
- **Chaos engineering hooks**: Instrument services to participate in chaos experiments; allow fault injection.
- **Service hooks and interceptors**: Middleware that intercepts requests/responses to add instrumentation.
- **Monitoring agents**: Lightweight sidecars or daemons that collect and export telemetry.

###  Observability in Microservices & SOA
- **Observability principles**: Understand internal state from external outputs; based on metrics, logs, traces.
- **Structured logging**: Logs in machine‑readable format (JSON) with fields for querying.
- **Correlation IDs**: Unique identifier passed across service calls to correlate logs and traces.
- **Distributed tracing**: Track request flow across services; identify bottlenecks and failures.
- **Metrics aggregation**: Collect and aggregate metrics from all services (Prometheus, StatsD).
- **Log aggregation**: Centralize logs from all services for search and analysis (ELK, Loki).
- **Monitoring dashboards**: Visualize key metrics (Grafana) to understand system health.
- **Alerting strategies**: Define alerts based on SLOs, error rates, latency; avoid alert fatigue.
- **Incident response**: Processes for detecting, diagnosing, resolving incidents; runbooks.
- **SLO/SLA tracking**: Measure service level objectives (uptime, latency) vs SLAs.
- **Error budgets**: Allowable error rate based on SLO; balance reliability with feature velocity.
- **Root cause analysis**: Investigate incidents to find underlying causes using traces and logs.
- **Production debugging**: Techniques like debuggers, log inspection, profiling in production.
- **Dependency mapping**: Discover and visualize service dependencies; understand impact of failures.

###  Microservices & SOA Architectures
- **Monolith to microservices migration**: Incremental strangler pattern; identify service boundaries.
- **Layered microservices architecture**: Services organized in layers (presentation, business, data) but distributed.
- **Event‑driven architecture**: Services communicate via events; decoupled and scalable.
- **Serverless microservices**: Functions as a service (FaaS) for certain workloads; auto‑scaling, pay‑per‑use.
- **Service mesh architecture**: Dedicated infrastructure layer for communication, security, observability.
- **Hexagonal microservices**: Ports and adapters; business logic isolated from external concerns.
- **Clean architecture in services**: Layers (entities, use cases, interfaces, infrastructure) separated.
- **API gateway architecture**: Single entry point for all clients; routes requests to appropriate services.
- **Backend‑for‑frontend**: Dedicated API gateways per client type (mobile, web) to optimize interactions.
- **Data mesh architecture**: Decentralized data ownership with domain‑oriented data products; federated governance.
- **Edge microservices**: Deploy services at network edge (CDN, edge nodes) for low latency.
- **Global microservices architecture**: Services deployed worldwide with data replication and global load balancing.
- **Multi‑region architecture**: Active‑active or active‑passive deployments across regions.
- **Hybrid architecture**: Mix of on‑prem and cloud; requires integration and consistent management.
- **Resilient architecture**: Design for failure; use redundancy, retries, circuit breakers, bulkheads.

###  Microservices & SOA Patterns
- **API gateway pattern**: Single entry point for clients; handles cross‑cutting concerns.
- **Sidecar pattern**: Deploy helper container alongside service for offloaded tasks (logging, proxy).
- **Circuit breaker**: Prevents cascading failures by opening circuit when failures threshold reached.
- **Retry pattern**: Retry failed operations with exponential backoff; ensure idempotency.
- **Bulkhead pattern**: Isolate resources into pools to limit failure impact.
- **Saga pattern**: Manage distributed transactions via sequence of local transactions with compensating actions.
- **CQRS pattern**: Separate read and write models; improve scalability and performance.
- **Event sourcing**: Store state as event log; rebuild state from events.
- **Strangler pattern**: Incrementally replace monolith by gradually migrating functionality.
- **Ambassador pattern**: Sidecar that handles communication with external services (retry, circuit breaking).
- **Aggregator pattern**: Service that aggregates data from multiple services into one response.
- **Service registry pattern**: Database of service instances and their locations; used for discovery.
- **Publisher/subscriber**: Asynchronous messaging where publishers emit events and subscribers react.
- **Competing consumers**: Multiple consumers process messages from a queue to scale processing.
- **Cache‑aside**: Load data into cache on read; update cache on write.
- **Backpressure pattern**: Downstream service signals load to upstream to slow down.

###  Microservices & SOA Anti‑Patterns
- **Distributed monolith**: Services deployed separately but tightly coupled, requiring coordinated changes.
- **Shared database**: Multiple services share the same database, leading to coupling and data ownership issues.
- **Tight coupling**: Services depend on internal details of others; changes ripple.
- **Chatty services**: Excessive communication between services; high latency, low cohesion.
- **Over‑granular services**: Too many tiny services; increased complexity, overhead.
- **God service**: One service does too much; becomes bottleneck and hard to maintain.
- **Synchronous dependency chains**: Long chains of synchronous calls; increased latency and failure risk.
- **Ignoring observability**: No monitoring, logging, tracing; hard to debug and operate.
- **No service ownership**: No team responsible; services become orphaned and degrade.
- **Lack of versioning**: APIs change without versioning; breaking clients.
- **Hardcoded service endpoints**: Config embedded in code; makes deployment and scaling difficult.
- **No failure isolation**: One service failure cascades to others; lack of circuit breakers, bulkheads.
- **Over‑centralized governance**: Strict standards hinder innovation and autonomy.
- **Cascade failures**: Failure propagates through system due to lack of resilience.
- **Hidden shared state**: Services unintentionally share state via caches or databases; leads to coupling.

###  Microservices & SOA Best Practices
- **Domain‑driven service design**: Align services with business domains and subdomains.
- **Contract‑first APIs**: Define API contracts upfront; use OpenAPI, gRPC proto.
- **Independent data ownership**: Each service owns its data; no shared databases.
- **Idempotent operations**: Ensure operations can be repeated safely.
- **Backward compatibility**: Maintain compatibility when evolving APIs.
- **Fail‑fast design**: Detect errors early and fail immediately; avoid masking issues.
- **Graceful degradation**: Provide reduced functionality when dependencies fail.
- **Observability‑first mindset**: Build monitoring, logging, tracing from the start.
- **Security by design**: Incorporate security in every layer; use mTLS, OAuth, API keys.
- **Automation‑first deployments**: Use CI/CD pipelines for automated testing and deployment.
- **Infrastructure as code**: Manage infrastructure via code (Terraform, CloudFormation).
- **CI/CD pipelines**: Automate build, test, and deployment stages.
- **Canary testing**: Gradually roll out changes to detect issues early.
- **Chaos engineering**: Proactively test resilience by injecting failures.
- **Service documentation**: Maintain up‑to‑date API docs, runbooks, architecture diagrams.
- **API governance**: Enforce standards and best practices across APIs.
- **Cost optimization**: Monitor and optimize resource usage; use spot instances, autoscaling.
- **Performance budgeting**: Set limits on latency, resource consumption; monitor against budgets.

###  Microservices & SOA Technologies
- **Container platforms**: Docker, container runtimes.
- **Kubernetes ecosystem**: Orchestration, service discovery, scaling, rolling updates.
- **Service mesh tools**: Istio, Linkerd, Consul Connect for traffic management, security, observability.
- **API gateways**: Kong, Apigee, AWS API Gateway, NGINX.
- **Message brokers**: RabbitMQ, ActiveMQ, Apache Kafka (event streaming).
- **Event streaming platforms**: Apache Kafka, AWS Kinesis, Pulsar.
- **Distributed tracing tools**: Jaeger, Zipkin, OpenTelemetry.
- **Logging platforms**: ELK stack (Elasticsearch, Logstash, Kibana), Loki, Splunk.
- **Monitoring platforms**: Prometheus, Grafana, Datadog, New Relic.
- **CI/CD systems**: Jenkins, GitLab CI, CircleCI, ArgoCD.
- **Infrastructure as code tools**: Terraform, Pulumi, AWS CloudFormation.
- **Serverless platforms**: AWS Lambda, Azure Functions, Google Cloud Functions.
- **Cloud microservices platforms**: AWS ECS/EKS, Azure Kubernetes Service, Google GKE.
- **Identity & auth systems**: OAuth2, OpenID Connect, Keycloak, Auth0.
- **Secrets management**: HashiCorp Vault, AWS Secrets Manager, Kubernetes Secrets.
- **Service registries**: Consul, Eureka, etcd, Zookeeper.
- **Distributed caches**: Redis, Memcached, Hazelcast.


# Arcitura Patterns
## SOA DESIGN PATTERNS

### Service Design Patterns
- **Service Contract Design** – Formalize service interfaces and interaction contracts.
- **Service Façade** – Expose simplified interface to hide underlying complexity.
- **Service Wrapper** – Adapt legacy systems to expose them as services.
- **Service Normalization** – Ensure data and service models conform to standards.
- **Service Decomposition** – Break down large services into smaller, cohesive ones.
- **Capability Composition** – Combine service capabilities to fulfill business processes.
- **Agnostic Service** – Business-agnostic, reusable service with generic logic.
- **Non-Agnostic Service** – Service tied to specific business process or domain.
- **Utility Service** – Provides technical/infrastructure capabilities (logging, messaging).
- **Entity Service** – Service representing a business entity (Customer, Order).
- **Task Service** – Service representing a business task or workflow step.

### Service Interaction Patterns
- **Service Composition** – Assemble services to create composite applications.
- **Orchestration** – Centralized control of service interactions.
- **Choreography** – Decentralized coordination via events.
- **Asynchronous Messaging** – Non‑blocking communication between services.
- **Competing Consumers** – Multiple consumers process messages from a single queue.
- **Event‑Driven Messaging** – Services communicate via events.
- **Callback** – Service provides a callback endpoint for async response.
- **Service Router** – Routes requests to appropriate service instances.

### Service Governance Patterns
- **Canonical Data Model** – Standardized data model for interoperability.
- **Contract Centralization** – Central management of service contracts.
- **Service Registry** – Central repository for service metadata and endpoints.
- **Policy Centralization** – Centralized management of policies (security, etc.).
- **SLA Centralization** – Central monitoring and enforcement of SLAs.

---

## MICROSERVICES PATTERNS

### Architectural Patterns
- **Bounded Context** – Domain‑driven design boundary for microservice.
- **API Gateway** – Single entry point for clients, routing, composition.
- **Database per Service** – Each service has its own database.
- **Decentralized Governance** – Teams choose their own tech stacks.
- **Backend for Frontend (BFF)** – Dedicated backend per frontend client.
- **Service Registry** – Database of service instances and locations.
- **Service Discovery** – Dynamic finding of service instances.

### Communication Patterns
- **Synchronous REST** – HTTP/REST for request‑response.
- **Asynchronous Messaging** – Message queues for decoupled communication.
- **Event Streaming** – Kafka‑like streams for event propagation.
- **Pub/Sub** – Publish‑subscribe model for events.

### Data Patterns
- **CQRS** – Separate read and write models.
- **Event Sourcing** – Store state as sequence of events.
- **Saga** – Distributed transaction coordination via compensating actions.
- **API Composition** – Aggregate data from multiple services.
- **Aggregator** – Service that aggregates responses from multiple services.

### Resiliency Patterns
- **Circuit Breaker** – Prevent cascading failures by failing fast.
- **Retry** – Automatic retry of failed operations.
- **Timeout** – Limit wait time for responses.
- **Bulkhead** – Isolate resources to limit failure impact.
- **Idempotent Receiver** – Ensure operations can be safely retried.

---

## CLOUD PATTERNS

### Cloud Architecture Patterns
- **Elastic Resource Capacity** – Dynamically scale resources based on demand.
- **Resource Pooling** – Share resources among multiple users.
- **Resource Replication** – Duplicate resources for availability.
- **Dynamic Scalability** – Automatic scaling up/down.
- **Failover System** – Automatic switch to standby on failure.
- **Load Balanced Virtual Server** – Distribute load across servers.
- **Redundant Storage** – Store data redundantly for durability.
- **Audit Monitor** – Track resource usage for compliance.
- **Usage Monitor** – Monitor resource consumption.
- **Pay‑Per‑Use Monitor** – Track usage for billing.

### Cloud Security Patterns
- **Identity Federation** – Federated identity management across providers.
- **Encrypted Storage** – Data encryption at rest.
- **Secure Communication** – Encryption in transit.
- **Multi‑Tenancy Isolation** – Isolate tenants in shared infrastructure.

### Cloud Operational Patterns
- **Automated Scaling Listener** – Triggers scaling based on metrics.
- **Resource Reservation** – Reserve capacity for future use.
- **Cloud Balancing** – Distribute workload across clouds.

---

## BIG DATA PATTERNS

### Data Architecture Patterns
- **Data Lake** – Raw data storage in native format.
- **Data Warehouse** – Structured, processed data for analytics.
- **Data Lakehouse** – Combines data lake and warehouse features.
- **Data Mesh** – Decentralized data ownership and architecture.
- **Schema‑on‑Read** – Apply schema when reading data.
- **Schema‑on‑Write** – Apply schema when writing data.

### Processing Patterns
- **Batch Processing** – Process large volumes in batches.
- **Stream Processing** – Real‑time processing of data streams.
- **Micro‑Batch Processing** – Small batches for near‑real‑time.
- **Real‑Time Analytics** – Immediate analysis of incoming data.

### Governance Patterns
- **Data Lineage** – Track data origin and transformations.
- **Metadata Centralization** – Central metadata management.
- **Master Data Management** – Manage core business entities consistently.
- **Data Quality Monitoring** – Monitor and ensure data quality.

---

## DEVOPS PATTERNS

### Pipeline Patterns
- **Continuous Integration** – Frequent integration of code changes.
- **Continuous Delivery** – Automate deployment to staging, manual to prod.
- **Continuous Deployment** – Automate deployment to production.
- **Pipeline as Code** – Define CI/CD pipelines in code.

### Infrastructure Patterns
- **Infrastructure as Code** – Manage infrastructure via code.
- **Immutable Infrastructure** – Replace servers rather than update.
- **Configuration as Code** – Manage configurations in code.
- **Environment Replication** – Replicate environments for consistency.

### Release Patterns
- **Blue‑Green Deployment** – Two environments, switch traffic.
- **Canary Release** – Gradual rollout to subset of users.
- **Rolling Deployment** – Incremental update of instances.

### Observability Patterns
- **Centralized Logging** – Aggregate logs for analysis.
- **Distributed Tracing** – Trace requests across services.
- **Health Monitoring** – Monitor system health.
- **Error Budget Governance** – Manage reliability vs features.

---

## SECURITY PATTERNS (Cross‑Domain)
- **Zero Trust Model** – Never trust, always verify.
- **RBAC** – Role‑Based Access Control.
- **Token‑Based Authentication** – Use tokens for auth (JWT).
- **OAuth2** – Authorization framework.
- **API Security Gateway** – Secure API access.
- **Secrets Management** – Securely store and manage secrets.
- **Encryption at Rest** – Encrypt stored data.
- **Encryption in Transit** – Encrypt data in transit.

---

## GOVERNANCE & ENTERPRISE PATTERNS
- **Architecture Review Board** – Governance body for architecture decisions.
- **Policy Enforcement** – Enforce policies across organization.
- **Centralized Monitoring** – Central oversight of systems.
- **Compliance Automation** – Automate compliance checks.
- **Risk Mitigation Framework** – Systematic risk management.
- **Service Portfolio Management** – Manage services as a portfolio.
- **Platform Engineering Model** – Build internal developer platforms.


