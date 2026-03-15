# Microservices & SOA — Senior Developer Interview Notes

> **Format:** Dense study notes · Diagrams in ASCII/Mermaid · Interview-ready depth

---

# 1. Microservices & SOA Fundamentals

## Microservices vs SOA

| Dimension | SOA | Microservices |
|---|---|---|
| Scope | Enterprise-wide | Single bounded domain |
| Communication | ESB (Enterprise Service Bus) | Lightweight (HTTP/gRPC/MQ) |
| Data | Shared canonical data model | Independent DB per service |
| Deployment | Coarse-grained, shared runtime | Fine-grained, independent |
| Governance | Central, standardized | Decentralized, team-owned |
| Size | Large services | Small, single-purpose |
| Coupling | Often tightly coupled via ESB | Loosely coupled |

**Key Insight:** Microservices are SOA done right — remove the ESB, shrink service scope, enforce data autonomy.

---

## Evolution of Distributed Architectures

```
Monolith → SOA (ESB) → Microservices → Serverless/Service Mesh
   ↑           ↑              ↑                ↑
Simple    Reuse+Integrate  Autonomy+Scale   Functions+Infra abstraction
```

- **Monolith:** Single deployable, single DB. Easy to start, hard to scale independently.
- **SOA:** Services communicate via ESB. ESB becomes a bottleneck and SPOF.
- **Microservices:** No ESB. Smart endpoints, dumb pipes. Each service owns its data.
- **Service Mesh / Serverless:** Infrastructure handles cross-cutting concerns (mTLS, retries, routing).

---

## Service Boundaries & Bounded Contexts

**Bounded Context (DDD):** A linguistic boundary within which a model is consistent. The word "Account" means something different in Banking vs CRM — separate bounded contexts.

```
┌─────────────────────┐   ┌─────────────────────┐
│   Order Context     │   │   Payment Context   │
│  "Order" = purchase │   │ "Order" = tx record │
│  lifecycle entity   │   │  for charge/refund  │
└─────────────────────┘   └─────────────────────┘
       ↑ Anti-Corruption Layer ↑
```

**Rules for good boundaries:**
- One team owns one bounded context
- No cross-context DB joins
- Communication via well-defined APIs or events
- Domain language consistent within, intentionally translated across

---

## Domain-Driven Service Design

- Identify **Aggregates** → natural service boundaries
- **Entities** have identity across time; **Value Objects** don't
- **Domain Events** drive inter-service communication
- Avoid **Anemic Domain Models** (logic leaks into service layer)
- Apply **Ubiquitous Language** — code names match domain expert names

---

## Stateless Services

- No session state stored in service memory
- State lives in DB, cache (Redis), or passed in request
- Enables horizontal scaling — any instance handles any request
- Idempotency becomes natural
- **Exception:** Stateful streaming (Kafka Streams, Flink) — managed externally

---

## Service Autonomy & Granularity

**Autonomy:** A service can be deployed, scaled, and failed without impacting others.

**Granularity Trade-offs:**

```
Too Coarse                     Too Fine
──────────────────────────────────────────
Few services, tight coupling   Chatty network, ops overhead
Hard to scale independently    High latency, complex coordination
Slower teams                   Distributed monolith risk
```

**Goldilocks Rule:** One service = one business capability. If you frequently deploy two services together → merge. If one service does unrelated things → split.

---

## API Contracts

- Define API contract **before** implementation (contract-first)
- Use **OpenAPI/Swagger** for REST, **Protobuf** for gRPC, **AsyncAPI** for events
- Contracts are a team-to-team SLA
- **Consumer-Driven Contract Testing (CDCT):** Consumers define expectations; providers verify (Pact framework)

---

## Loose Coupling & High Cohesion

- **Loose Coupling:** Changes in Service A don't force changes in Service B
  - Avoid: shared DB, shared libraries with business logic, synchronous chains
  - Prefer: async events, well-versioned APIs, choreography
- **High Cohesion:** Everything inside a service relates to one purpose
  - Code that changes together belongs together

---

## Service Ownership (You Build It, You Run It)

- Team owns service from dev → prod → incident
- No handoff to ops team at deployment
- Drives accountability, quality, and observability mindset
- Conway's Law: System architecture mirrors org communication structure → design org to match desired architecture

---

## Service Lifecycle

```
Design → Build → Test → Deploy → Monitor → Deprecate
   ↑                                           ↓
   └───────── Iterate / Version ───────────────┘
```

---

## Service Discovery

**Client-Side Discovery:**
- Client queries Service Registry (Eureka, Consul)
- Client performs load balancing
- Example: Netflix Ribbon + Eureka

**Server-Side Discovery:**
- Client hits load balancer/router
- Router queries registry and forwards
- Example: AWS ALB + ECS, Kubernetes kube-proxy

```
Client-Side:                  Server-Side:
Client → Registry             Client → LB/Router → Registry
Client → Service Instance            ↓
                               Service Instance
```

---

## API Gateway Fundamentals

Single entry point for all clients. Handles:
- Routing to downstream services
- Auth/AuthZ (JWT validation, OAuth)
- Rate limiting & throttling
- SSL termination
- Request/response transformation
- Aggregation (fan-out and merge)
- Observability (access logs, metrics)

**Tools:** Kong, AWS API Gateway, NGINX, Envoy, Traefik

---

## Inter-Service Communication

### Synchronous
- **REST/HTTP:** Simple, human-readable, widely supported. Higher latency due to coupling.
- **gRPC:** Binary (Protobuf), HTTP/2, strong typing, bidirectional streaming. Best for internal services.
- **GraphQL:** Flexible querying. BFF layer often uses it.

### Asynchronous
- **Message Queue (RabbitMQ, SQS):** Point-to-point or pub/sub. Decouples producer/consumer.
- **Event Streaming (Kafka):** Persistent log, replay, high throughput. Event-driven at scale.

```
Sync:    ServiceA ──HTTP/gRPC──► ServiceB (blocks, waits)
Async:   ServiceA ──event──► Broker ◄──poll── ServiceB (non-blocking)
```

---

## Event-Driven Services

- Services react to domain events, not direct calls
- Temporal decoupling — producer and consumer don't need to be alive simultaneously
- Enables loose coupling at the system level
- Events are facts: `OrderPlaced`, `PaymentProcessed` — immutable

---

## Service Governance

- Standardized patterns for: logging, health checks, auth, versioning, error handling
- Enforced via: shared libraries (thin), service templates, linting/CI gates
- Avoid central ESB governance — too slow
- **Inner Source:** Teams share standards via documented conventions, not mandates

---

## Versioning Strategies

| Strategy | How | Trade-off |
|---|---|---|
| URI versioning | `/v1/orders` | Pollutes URL, easy to route |
| Header versioning | `Accept: application/vnd.api.v2+json` | Cleaner URL, harder to test in browser |
| Query param | `?version=2` | Simple, not RESTful |
| Content negotiation | Media type negotiation | Most RESTful |

**Rules:**
- Never break a published API
- Add fields (additive changes) = backward compatible
- Remove/rename fields = breaking → new version

---

## Backward Compatibility

- **Tolerant Reader Pattern:** Consumers ignore unknown fields
- **Postel's Law:** Be conservative in what you send, liberal in what you accept
- Use **feature flags** to roll out new contract behavior gradually
- **Sunset headers** to deprecate old versions

---

## Contract-First Design

1. Define API spec (OpenAPI/Protobuf/AsyncAPI) first
2. Stub out server and client from spec
3. Teams develop in parallel
4. Contract tests validate both sides
5. Deploy — no surprises

---

# 2. Advanced Microservices & SOA

## Distributed System Design — Core Properties

**Fallacies of Distributed Computing (Deutsch):**
1. Network is reliable
2. Latency is zero
3. Bandwidth is infinite
4. Network is secure
5. Topology doesn't change
6. There is one administrator
8. Transport cost is zero
9. Network is homogeneous

**Design for failure from day 1.**

---

## CAP Theorem

```
        Consistency
           /\
          /  \
         /    \
        /      \
       /        \
Availability ── Partition Tolerance
```

In a distributed system, during a network partition you choose:
- **CP (Consistency + Partition Tolerance):** Return error rather than stale data. E.g., HBase, Zookeeper
- **AP (Availability + Partition Tolerance):** Return possibly stale data. E.g., Cassandra, DynamoDB (eventual consistency)
- **CA (Consistency + Availability):** Only possible without partitions — not realistic in distributed systems

**Microservices default:** AP with eventual consistency. Design compensating flows.

---

## Data Consistency Models

| Model | Guarantee | Example |
|---|---|---|
| Strong consistency | All reads see latest write | RDBMS, etcd |
| Eventual consistency | Reads will converge eventually | DynamoDB, Cassandra |
| Causal consistency | Causally related operations ordered | MongoDB sessions |
| Read-your-writes | Writer sees own writes | CockroachDB |
| Monotonic reads | No going back in time | Spanner |

---

## Saga Pattern — Distributed Transactions

**Problem:** No 2PC in microservices (locks across services = deadly). Use Sagas.

**Saga:** A sequence of local transactions. Each step publishes an event or message. On failure, compensating transactions undo previous steps.

### Choreography-based Saga
```
OrderSvc ──OrderPlaced──► PaymentSvc ──PaymentProcessed──► InventorySvc
                                ↓ (on fail)
                        PaymentFailed ──► OrderSvc (OrderCancelled)
```
- No central coordinator
- Services react to events
- Harder to visualize global state

### Orchestration-based Saga
```
         ┌──────────────────────────────┐
         │       Saga Orchestrator       │
         │  (Order Process Manager)      │
         └──┬────────────┬──────────────┘
            │            │
       PaymentSvc    InventorySvc
```
- Central orchestrator commands each step
- Easier to track state, add retries, handle failures
- Orchestrator can become a bottleneck/SPOF if not designed carefully
- **Tools:** Temporal, Conductor, AWS Step Functions, Axon

---

## Event Sourcing

- Store **all state changes as events** (append-only log), not current state
- Reconstruct current state by replaying events
- Natural audit log, temporal queries, event replay for new projections

```
Event Store (append-only):
┌─────────────────────────────────────────────────────────┐
│ OrderCreated | ItemAdded | ItemAdded | PaymentConfirmed  │
└─────────────────────────────────────────────────────────┘
                    ↓ replay
              Current Order State
```

**Trade-offs:**
- ✅ Full audit trail, time travel, decoupled projections
- ❌ Event schema evolution is hard, eventual consistency, high storage

---

## CQRS (Command Query Responsibility Segregation)

Separate read and write models.

```
          Write Side                   Read Side
┌────────────────────┐       ┌────────────────────────┐
│  Command Handler   │──────►│  Projection / Read DB  │
│  (Domain Model)    │ event │  (Denormalized, fast)  │
│  Write DB          │       │  Query Handler         │
└────────────────────┘       └────────────────────────┘
```

- Write model = normalized, consistent, transactional
- Read model = denormalized, optimized for specific queries, possibly stale
- Often combined with Event Sourcing (ES+CQRS)
- **Use when:** Complex domain, high read/write ratio difference, different scaling needs

---

## Idempotency Design

- An operation applied multiple times produces the same result
- Critical for: retries, at-least-once message delivery, network timeouts
- **Idempotency Key:** Client sends unique key; server deduplicates
- **Natural idempotent ops:** PUT (set), DELETE (already gone = OK)
- **Non-natural:** POST (create) — add idempotency key header

```
POST /payments
Idempotency-Key: uuid-abc-123
→ First call: process and store result keyed to uuid
→ Retry: return cached result, don't reprocess
```

---

## Exactly-Once Processing

- **At-most-once:** Fire and forget. May lose messages.
- **At-least-once:** Retry until ACK. May duplicate.
- **Exactly-once:** Transactional idempotent processing. Hard to achieve end-to-end.

Kafka approach: **Transactional Producer + Idempotent Consumer**
- Producer: `enable.idempotence=true`, transactional API
- Consumer: read-process-commit in a DB transaction (outbox pattern)

---

## Service Mesh Architecture

Offloads cross-cutting concerns from application code to infrastructure.

```
┌─────────────────────────────────────────────────────┐
│                   Service Mesh                       │
│  ┌──────────┐      ┌──────────┐      ┌──────────┐  │
│  │ Service A│      │ Service B│      │ Service C│  │
│  │ [sidecar]│◄────►│ [sidecar]│◄────►│ [sidecar]│  │
│  └──────────┘      └──────────┘      └──────────┘  │
│                        ↑                            │
│                   Control Plane                     │
│              (Istio / Linkerd / Consul)              │
└─────────────────────────────────────────────────────┘
```

**Provides:** mTLS, load balancing, circuit breaking, retries, distributed tracing, traffic splitting — **without code changes**.

**Tools:** Istio (most feature-rich), Linkerd (lightweight, Rust-based), Consul Connect

---

## Sidecar Pattern

- A secondary container deployed alongside the main service container
- Handles: proxy, logging agent, secret injection, health monitoring
- Shares network namespace (localhost communication)
- **Envoy proxy** is the dominant sidecar implementation

---

## Multi-Region / Multi-Cloud Architecture

```
Region A (Primary)          Region B (Secondary)
┌──────────────────┐        ┌──────────────────┐
│  Services        │◄──────►│  Services        │
│  DB Primary      │  async │  DB Replica      │
│  Cache           │ repl.  │  Cache           │
└──────────────────┘        └──────────────────┘
         ↑                          ↑
         └──────── Global LB ───────┘
```

- **Active-Passive:** One region handles traffic; other is hot standby
- **Active-Active:** Both regions serve traffic; conflicts resolved via CRDTs or last-write-wins
- **Data residency:** GDPR/compliance constraints → data must stay in region
- **Tools:** AWS Route 53 latency routing, Cloudflare, Akamai GTM

---

## Zero-Downtime Deployments

### Blue-Green
```
Load Balancer
     │
     ├──► Blue (v1, current)  ← live traffic
     └──► Green (v2, new)     ← deploy + test
          ↓ (switch)
Load Balancer
     │
     └──► Green (v2) ← all traffic
          Blue kept for rollback
```

### Canary Release
```
100% traffic → v1
   ↓ shift 5%
95% → v1, 5% → v2  ← monitor metrics
   ↓ shift 25%
75% → v1, 25% → v2
   ↓ promote
100% → v2
```

### Feature Flags
- Deploy code dark, toggle features via config
- Decouple deployment from release
- Tools: LaunchDarkly, Unleash, Flagsmith
- Use for: A/B testing, kill switch, gradual rollout, trunk-based development

---

# 3. Performance in Microservices

## Latency in Distributed Systems

**Key metrics:**
- **p50 (median):** Half of requests faster than this
- **p95/p99:** Tail latency — worst-case for most users
- **p999:** Extreme outliers — important for SLA

**Sources of latency:**
- Network round-trips (each hop adds ~1ms LAN, ~50-100ms WAN)
- Serialization/deserialization
- DB queries
- Garbage collection pauses
- Synchronous dependency chains (each hop multiplies latency)

**Formula for chain latency:**
```
Total latency ≥ sum of all synchronous service calls
+ network overhead × number of hops
```

---

## Network Overhead Optimization

- Use **gRPC** (binary, HTTP/2 multiplexing) over REST for internal calls
- **HTTP Keep-Alive** / connection reuse
- **Compression:** gzip/Brotli for REST payloads
- **Batching:** Group small requests into one call
- **Async where possible:** Remove synchronous hops from critical path

---

## Load Balancing Strategies

| Strategy | How | Best For |
|---|---|---|
| Round Robin | Rotate requests evenly | Stateless, homogeneous services |
| Least Connections | Route to instance with fewest active | Long-lived connections |
| Weighted | Route more to powerful instances | Heterogeneous capacity |
| IP Hash | Same client → same instance | Session affinity |
| Random with 2 choices (P2C) | Pick best of 2 random | Reduces hotspots vs round-robin |

---

## Connection Pooling

- Maintain a pool of pre-established connections (DB, HTTP, gRPC)
- Avoids TCP handshake overhead per request
- **DB pool sizing:** `pool_size = (core_count * 2) + effective_spindle_count` (HikariCP formula)
- Monitor: pool exhaustion = latency spikes

---

## Caching Strategies

```
         Request
            │
      ┌─────▼─────┐
      │  Cache?   │ Yes → return
      └─────┬─────┘
            │ No (miss)
      ┌─────▼─────┐
      │  Service  │
      └─────┬─────┘
            │
      ┌─────▼─────┐
      │  Write to │
      │   Cache   │
      └───────────┘
```

| Strategy | Description | Use Case |
|---|---|---|
| Cache-Aside (Lazy) | App checks cache, loads on miss | General purpose |
| Write-Through | Write to cache + DB simultaneously | Strong consistency |
| Write-Behind | Write to cache, async flush to DB | High write throughput |
| Read-Through | Cache fetches from DB on miss | Simplifies app code |
| Refresh-Ahead | Proactively refresh before expiry | Predictable access patterns |

**TTL discipline:** Too short = cache misses. Too long = stale data.

---

## Bulkheads

Isolate failures. Each service/thread pool has dedicated resources — one failing consumer doesn't exhaust shared pool.

```
Without Bulkhead:               With Bulkhead:
[Shared Thread Pool]            [Pool A: ServiceA]
  All services share              ServiceB slow? Only its pool fills
  ServiceB slow? Blocks all       ServiceA unaffected
```

**Hystrix / Resilience4j** implement bulkheads via semaphores or thread pool isolation.

---

## Rate Limiting & Throttling

- **Rate limiting:** Max N requests per time window per client
- **Throttling:** Slow down rather than reject (queue + delay)
- **Algorithms:**
  - **Token Bucket:** Refill tokens at rate R; each request consumes 1. Allows bursts.
  - **Leaky Bucket:** Requests drain at fixed rate. Smooths bursts.
  - **Fixed Window:** Count requests per window. Boundary burst problem.
  - **Sliding Window:** Rolling window, more accurate. Higher memory.

---

## Backpressure

Signal to producers to slow down when consumers are overwhelmed.

```
Producer ──messages──► Consumer (slow)
                         │
         ◄── "slow down" │ (backpressure)
```

- **Reactive Streams** (Project Reactor, RxJava): Built-in backpressure protocol
- **Kafka:** Consumer controls poll rate; producer doesn't overflow
- **TCP flow control:** OS-level backpressure
- Without it: memory overflow, OOM, cascading failure

---

## Autoscaling Strategies

- **Horizontal Pod Autoscaler (HPA):** Scale on CPU/memory/custom metrics
- **Vertical Pod Autoscaler (VPA):** Adjust resource requests/limits
- **KEDA (Kubernetes Event-Driven Autoscaler):** Scale to zero based on queue depth, Kafka lag, etc.
- **Predictive scaling:** Use historical patterns (AWS Predictive Scaling)
- **Scale-down caution:** Draining in-flight requests before termination (preStop hook, terminationGracePeriodSeconds)

---

## Tail Latency Reduction

- **Hedged requests:** Send same request to 2 instances simultaneously; use first response (Google SRE technique)
- **Timeout tuning:** Aggressive timeouts prevent slow services from blocking threads
- **GC tuning:** Use G1GC / ZGC to reduce pause times (Java)
- **Avoid noisy neighbor:** Resource limits and namespaces in Kubernetes
- **Warm-up:** Pre-warm JVM/connection pool after deployment

---

# 4. Instrumentation in Microservices

## Runtime Instrumentation

- **Auto-instrumentation:** Java agents (OpenTelemetry agent), bytecode manipulation
- **Manual instrumentation:** SDK calls in code (`tracer.startSpan(...)`)
- Instrument at: HTTP layer, DB calls, queue publish/consume, external API calls

---

## Metrics Collection

**Three types of metrics (RED + USE):**

**RED (Request-centric):**
- **R**ate: Requests per second
- **E**rrors: Error rate (%)
- **D**uration: Latency distribution

**USE (Resource-centric):**
- **U**tilization: % resource busy
- **S**aturation: Queue depth, wait time
- **E**rrors: Error events

**Tools:**
- **Prometheus:** Pull-based scraping, PromQL, time-series DB
- **StatsD / Graphite:** Push-based, UDP, low overhead
- **Micrometer (Java):** Vendor-neutral metrics facade

---

## Distributed Tracing Instrumentation

- Inject **Trace ID** + **Span ID** into every request
- Propagate via HTTP headers (`traceparent` — W3C standard, or `X-B3-TraceId` — Zipkin)
- Each service creates child spans
- Spans contain: operation name, timing, tags, logs, status

```
TraceID: abc123
  Span: API Gateway (0-50ms)
    Span: OrderService (5-30ms)
      Span: DB Query (10-20ms)
    Span: PaymentService (30-45ms)
```

**Tools:** Jaeger, Zipkin, AWS X-Ray, Tempo + Grafana

---

## Health Probes (Kubernetes)

```yaml
livenessProbe:   # Is the service alive? Restart if fails.
  httpGet:
    path: /health/live
    
readinessProbe:  # Is service ready to receive traffic? Remove from LB if fails.
  httpGet:
    path: /health/ready
    
startupProbe:    # Did service start up? Prevents liveness kill during slow start.
  httpGet:
    path: /health/startup
```

- **Liveness:** Checks if process is healthy (not deadlocked)
- **Readiness:** Checks if dependencies (DB, cache) are reachable
- **Startup:** Gives slow-starting services time (replaces initialDelaySeconds)

---

## Telemetry Pipelines

```
Services
  │
  ▼ (OTel SDK)
OpenTelemetry Collector
  │
  ├──► Metrics → Prometheus/Thanos
  ├──► Traces → Jaeger/Tempo
  └──► Logs → Loki/Elasticsearch
```

**OpenTelemetry (OTel):** Vendor-neutral, CNCF standard. Unified SDK for metrics, traces, logs.

---

## Chaos Engineering Hooks

- **Failure injection points:** Network latency, packet drop, CPU stress, memory pressure, service kill
- **Tools:** Chaos Monkey (Netflix), Litmus (K8s-native), Gremlin, AWS Fault Injection Simulator
- **Chaos experiments:** Define steady state → inject failure → observe → improve
- Build in `/chaos` endpoints or use service mesh traffic policies

---

# 5. Observability in Microservices

## The Three Pillars + One More

```
┌─────────────┬───────────────────────────────────────┐
│  Pillar     │  Purpose                              │
├─────────────┼───────────────────────────────────────┤
│  Metrics    │  "Is it slow / down?" — aggregated    │
│  Logs       │  "What happened?" — discrete events   │
│  Traces     │  "Where did it happen?" — request path│
│  Profiles   │  "Why is it slow?" — code-level perf  │
└─────────────┴───────────────────────────────────────┘
```

**Observability ≠ Monitoring.** Monitoring tells you when something is wrong. Observability lets you understand WHY — even for issues you didn't anticipate.

---

## Structured Logging

- Log as JSON, not plain text
- Include: `timestamp`, `level`, `service`, `traceId`, `spanId`, `requestId`, `userId`, `message`, `error`

```json
{
  "timestamp": "2025-01-01T12:00:00Z",
  "level": "ERROR",
  "service": "order-service",
  "traceId": "abc123",
  "spanId": "def456",
  "orderId": "ord-789",
  "message": "Payment validation failed",
  "error": "Insufficient funds"
}
```

**Tools:** Logback + Logstash encoder (Java), Winston (Node), structlog (Python)

---

## Correlation IDs

- Generate at API Gateway / edge
- Propagate via HTTP header (`X-Correlation-ID`, `X-Request-ID`)
- Include in every log entry and span
- Enables full request trace across all services from a single ID

---

## Distributed Tracing (Deep)

**Concepts:**
- **Trace:** End-to-end request journey
- **Span:** Single operation within a trace
- **Context propagation:** W3C `traceparent` header
- **Sampling:** 100% tracing is expensive; use adaptive/tail-based sampling

**Tail-Based Sampling:** Decide to keep/discard a trace **after** it's complete — keeps errors/slow traces, drops healthy ones.

---

## SLO / SLA / Error Budgets

```
SLA: External contract with penalties (e.g., 99.9% uptime)
SLO: Internal target (e.g., 99.95% — stricter than SLA)
SLI: The actual measured metric (e.g., % of successful requests)

Error Budget = 100% - SLO
  99.9% SLO → 0.1% error budget → ~8.7 hours/year downtime allowed

Error Budget Policy:
  Budget > 50%: Deploy freely
  Budget < 50%: Slow down deployments
  Budget depleted: Freeze all non-reliability changes
```

---

## Root Cause Analysis (RCA)

**5 Whys technique:** Keep asking "why" until root cause found, not symptom.

**Post-mortem structure (blameless):**
1. Incident timeline
2. Impact (users affected, revenue, SLO burn)
3. Root cause(s)
4. Contributing factors
5. What went well
6. Action items with owners and dates

---

## Dependency Mapping

- Visualize service-to-service call graph
- Identify: critical path, single points of failure, unexpected dependencies
- Tools: **AWS X-Ray Service Map**, Jaeger dependency graph, **Kiali** (Istio)

---

# 6. Microservices Architectures

## Monolith to Microservices Migration

**Strangler Fig Pattern:**
```
Step 1: New requests → route some to new microservice
        Old traffic still → monolith
Step 2: Gradually increase routing to new service
Step 3: Monolith code path retired
```

**Migration strategies:**
1. Identify seams (natural boundaries in monolith)
2. Extract read-only services first (lower risk)
3. Use feature flags to toggle monolith vs microservice
4. Sync data with dual-write or CDC (Change Data Capture)
5. Anti-corruption layer to translate models

---

## Event-Driven Architecture

```
Producer         Broker           Consumer
   │              │                  │
   ├─OrderPlaced──►│                  │
   │              │──OrderPlaced─────►├── Payment processing
   │              │──OrderPlaced─────►├── Email notification
   │              │──OrderPlaced─────►└── Analytics
```

- **Topics/Queues:** Kafka topics, SQS queues, RabbitMQ exchanges
- **Fan-out:** One event, many consumers (pub/sub)
- **Event schema registry:** Confluent Schema Registry — enforce Avro/Protobuf schemas

---

## Backend-for-Frontend (BFF)

```
Mobile Client ──► Mobile BFF ──► Downstream Services
Web Client    ──► Web BFF    ──►      (same)
3rd Party     ──► Public API ──►      (same)
```

- Each client type gets a tailored API
- BFF aggregates, transforms, and adapts responses for its specific client
- Avoids over-fetching / under-fetching for each client
- Team owning the frontend owns the BFF

---

## Hexagonal Architecture (Ports & Adapters)

```
         ┌──────────────────────────┐
 HTTP  ──►│  Input Port (REST)       │
 CLI   ──►│  ┌──────────────────┐   │
 MQ    ──►│  │   Domain Core    │   │──► DB (Output Port)
         │  │   (Use Cases)    │   │──► Email (Output Port)
         │  └──────────────────┘   │──► Queue (Output Port)
         └──────────────────────────┘
```

- **Domain core** has zero dependencies on frameworks/infrastructure
- **Ports:** Interfaces defined by domain
- **Adapters:** Implementations of ports (REST controller, JPA repo)
- Testable domain logic without infra dependencies

---

## Data Mesh Architecture

- Treat data as a product
- Domain teams own their data products (not a central data team)
- **Four principles:** Domain ownership, data as product, self-serve platform, federated governance
- Each domain exposes a well-defined data product (event stream, API, dataset)

---

## Resilient Architecture Patterns

```
Circuit Breaker States:
CLOSED ──(failures exceed threshold)──► OPEN
OPEN   ──(timeout elapsed)──► HALF-OPEN
HALF-OPEN ──(success)──► CLOSED
HALF-OPEN ──(failure)──► OPEN
```

- **Timeout + Retry + Circuit Breaker + Fallback** = standard resilience stack
- **Fallback:** Return cached data, default response, or gracefully degrade feature
- **Tools:** Resilience4j (Java), Polly (.NET), Hystrix (deprecated)

---

# 7. Microservices Patterns

## API Gateway Pattern
- Single entry point, handles routing, auth, rate limiting, aggregation
- **Avoid:** Business logic in gateway (keep it thin)
- **Gateway Aggregation:** Fan out to multiple services, merge response

## Sidecar Pattern
- Deploy helper container with main container (shared pod in K8s)
- Handles: proxying (Envoy), log shipping, secret refresh, health checking
- App code stays clean

## Circuit Breaker
- Fail fast when downstream is unhealthy
- Prevents thread pool exhaustion, cascading failures
- States: CLOSED (normal) → OPEN (failing, reject calls) → HALF-OPEN (probe recovery)

## Retry Pattern
- Retry transient failures with exponential backoff + jitter
- `retry_delay = min(cap, base * 2^attempt) + random_jitter`
- **Always idempotent operations only** — never retry non-idempotent without deduplication

## Saga Pattern
- (See Section 2 — Orchestration vs Choreography)

## CQRS Pattern
- (See Section 2)

## Strangler Fig Pattern
- Incrementally replace monolith features
- New capability in microservice, old in monolith
- Route traffic gradually

## Ambassador Pattern
- Helper service that proxies network calls from main service
- Adds: retry, circuit breaking, routing, SSL offload
- Common in legacy service modernization

## Aggregator Pattern
- Service calls multiple downstream services and aggregates results
- Can be: **parallel** (scatter-gather) or **chained**
- Used by BFF layer

## Service Registry Pattern
- Central directory of available service instances
- **Self-registration:** Service registers itself on startup (Eureka)
- **Third-party registration:** Orchestrator (K8s) registers service

## Publisher/Subscriber
- Publisher sends event to topic
- Subscribers independently consume
- Decoupled, scalable, async

## Competing Consumers
- Multiple consumers read from same queue
- Distribute workload — scale consumers independently
- Ensure idempotency (message may be delivered to multiple in edge cases)

## Cache-Aside
- (See Section 3 — Caching Strategies)

## Outbox Pattern (for Exactly-Once with messaging)
```
Within one DB transaction:
1. Write business entity
2. Write event to OUTBOX table

Separate relay process:
3. Poll OUTBOX → publish to broker → mark published
```
Guarantees: event published if and only if business transaction committed.

---

# 8. Anti-Patterns

| Anti-Pattern | Problem | Fix |
|---|---|---|
| **Distributed Monolith** | Services deployed together, shared state | Define true bounded contexts, decouple data |
| **Shared Database** | Schema change in one service breaks others | DB per service, communicate via API/events |
| **Tight Coupling** | Compile-time / runtime dependencies everywhere | Use async events, interface contracts |
| **Chatty Services** | N+1 calls across services | Aggregate queries, BFF, async batch |
| **Over-granular Services** | 50 services for a simple domain | Merge services with high co-deployment frequency |
| **God Service** | One service does everything | Split by bounded context |
| **Synchronous Dependency Chains** | A→B→C→D all sync; one slow = all slow | Break chains with async, caching |
| **Ignoring Observability** | Can't debug production | Instrument from day 1 |
| **No Service Ownership** | Nobody knows who to call | Assign team ownership per service |
| **Hardcoded Endpoints** | Service A hardcodes IP of B | Use service registry / DNS |
| **Cascade Failures** | One service down takes down system | Circuit breakers, bulkheads, timeouts |
| **Hidden Shared State** | Services share in-memory caches | No shared state; use dedicated cache service |
| **No Failure Isolation** | All services in one thread pool | Bulkheads, resource limits |
| **Over-centralized Governance** | All changes need central approval | Federated governance with standards |

---

# 9. Best Practices

## Domain-Driven Service Design
- Start with Event Storming workshops — surface domain events, aggregates, bounded contexts
- Map bounded contexts to services 1:1 (or 1:few for very large contexts)

## Contract-First APIs
- Define OpenAPI/Protobuf spec first
- Generate stubs from spec
- Run CDCT (Pact) in CI to catch breaking changes before deployment

## Independent Data Ownership
- Each service owns its schema — no cross-service DB joins
- Share data via APIs or events
- If you must join data, use a read model / materialized view maintained by events

## Fail-Fast Design
- Validate inputs at the boundary
- Set aggressive timeouts on downstream calls
- Return early on error — don't buffer failures

## Graceful Degradation
- Identify non-critical features per service
- Return partial/cached responses when dependencies fail
- Use feature flags to disable non-essential features under load

## Observability-First
- Instrument before writing business logic
- Define SLOs before going to production
- Ensure every request has a trace ID at service boundary

## Security by Design
- **mTLS** between services (service mesh makes this easy)
- **Zero-trust networking** — services authenticate each other, not just at perimeter
- **Least privilege IAM** — each service has its own identity
- **Secrets management:** Vault, AWS Secrets Manager — never secrets in env vars or code
- **Input validation** at every service boundary

## CI/CD Pipelines for Microservices
```
Code Push
  │
  ├── Unit Tests
  ├── Contract Tests (Pact)
  ├── Integration Tests
  ├── Build Container Image
  ├── Security Scan (Snyk, Trivy)
  ├── Push to Registry
  └── Deploy (Canary → Full)
        │
        └── Automated rollback on error budget breach
```

## Infrastructure as Code
- **Terraform:** Provision cloud resources declaratively
- **Helm:** Package K8s manifests
- **Kustomize:** Environment-specific K8s overlays
- **GitOps (ArgoCD / Flux):** Git is single source of truth; sync cluster to repo

## Chaos Engineering (Maturity Model)
```
Level 1: Manual failover tests
Level 2: Automated chaos experiments in staging
Level 3: Continuous chaos in production with auto-rollback
Level 4: Chaos integrated into CI/CD pipeline
```

## Cost Optimization
- Right-size containers (VPA recommendations)
- Use Spot/Preemptible instances for stateless services
- Scale to zero for non-critical workloads (KEDA)
- Monitor per-service cost attribution

---

# 10. Technologies Reference

## Container Platforms
| Tool | Purpose |
|---|---|
| Docker | Container build + local runtime |
| Kubernetes (K8s) | Container orchestration, scheduling, self-healing |
| EKS / GKE / AKS | Managed K8s (AWS / GCP / Azure) |
| OpenShift | Enterprise K8s with extra security/tooling |

## Kubernetes Ecosystem
- **Helm:** Package manager for K8s
- **Kustomize:** Config layering without templating
- **ArgoCD / Flux:** GitOps continuous delivery
- **KEDA:** Event-driven autoscaling
- **Cert-Manager:** Automatic TLS cert management
- **Sealed Secrets / External Secrets:** Secret management in K8s

## Service Mesh Tools
| Tool | Notes |
|---|---|
| Istio | Feature-rich, Envoy-based, control plane: istiod |
| Linkerd | Lightweight, Rust proxy, CNCF graduated |
| Consul Connect | HashiCorp, multi-platform, service registry built-in |
| AWS App Mesh | Managed mesh, Envoy-based |

## API Gateways
| Tool | Notes |
|---|---|
| Kong | Open-source, plugin-based, Lua/Go |
| AWS API Gateway | Managed, tight AWS integration |
| NGINX | Reverse proxy + gateway |
| Envoy | High-performance proxy, L7 |
| Traefik | Cloud-native, automatic service discovery |
| Apigee | Enterprise, Google Cloud |

## Message Brokers & Event Streaming
| Tool | Notes |
|---|---|
| Apache Kafka | High-throughput, durable, event log, replay |
| RabbitMQ | AMQP, flexible routing, lower throughput vs Kafka |
| AWS SQS / SNS | Managed queue / pub-sub |
| Azure Service Bus | Enterprise messaging, sessions, deduplication |
| NATS | Lightweight, cloud-native, low-latency |
| Pulsar | Multi-tenancy, tiered storage, Kafka alternative |

## Distributed Tracing
| Tool | Notes |
|---|---|
| Jaeger | CNCF, open-source, Uber origin |
| Zipkin | Twitter origin, simple |
| AWS X-Ray | Managed, integrates with Lambda/ECS |
| Grafana Tempo | Scalable, integrates with Loki/Prometheus |
| OpenTelemetry Collector | Vendor-neutral SDK + pipeline |

## Logging Platforms
| Tool | Notes |
|---|---|
| ELK Stack | Elasticsearch + Logstash + Kibana |
| EFK Stack | Elasticsearch + Fluentd + Kibana |
| Grafana Loki | Log aggregation, label-based (cheaper than ELK) |
| AWS CloudWatch Logs | Managed, tight AWS integration |
| Datadog | SaaS, unified metrics+logs+traces |

## Monitoring & Alerting
| Tool | Notes |
|---|---|
| Prometheus | Pull-based metrics, PromQL |
| Grafana | Dashboards, alerting |
| Thanos / Cortex | Prometheus HA + long-term storage |
| Datadog | SaaS, full observability |
| New Relic | SaaS APM |
| PagerDuty / OpsGenie | Incident alerting + on-call management |

## CI/CD Systems
| Tool | Notes |
|---|---|
| GitHub Actions | Native GitHub, YAML workflows |
| GitLab CI | Built-in, powerful, self-hostable |
| Jenkins | Mature, plugin-rich, self-hosted |
| Tekton | K8s-native CI/CD pipelines |
| ArgoCD | GitOps CD for K8s |
| CircleCI | SaaS, fast, Docker-native |

## Infrastructure as Code
| Tool | Notes |
|---|---|
| Terraform | Multi-cloud provisioning |
| Pulumi | IaC with real programming languages |
| AWS CloudFormation | AWS-native IaC |
| Ansible | Configuration management, not just IaC |
| Crossplane | K8s-native cloud resource provisioning |

## Serverless Platforms
| Tool | Notes |
|---|---|
| AWS Lambda | Event-driven functions |
| Google Cloud Functions | GCP equivalent |
| Azure Functions | Azure equivalent |
| Knative | K8s-based serverless |
| OpenFaaS | Self-hosted serverless on K8s |

## Identity, Auth & Secrets
| Tool | Notes |
|---|---|
| Keycloak | Open-source IAM, OIDC/OAuth2 |
| Auth0 / Okta | SaaS IAM |
| AWS Cognito | Managed user pools |
| HashiCorp Vault | Secrets management, dynamic credentials |
| AWS Secrets Manager | Managed secrets, auto-rotation |
| SPIFFE/SPIRE | Service identity, workload attestation |

## Distributed Caches
| Tool | Notes |
|---|---|
| Redis | In-memory, data structures, pub/sub, Lua scripting |
| Memcached | Simple, fast, no persistence |
| AWS ElastiCache | Managed Redis / Memcached |
| Apache Ignite | In-memory compute grid |
| Hazelcast | Distributed computing platform |

## Service Registries
| Tool | Notes |
|---|---|
| Consul | Service discovery + config + mesh |
| Eureka | Netflix OSS, Spring Cloud native |
| etcd | Key-value store, K8s backbone |
| Zookeeper | Coordination service (Kafka uses it / moving to KRaft) |

---

# Quick-Reference: Interview Cheat Sheet

## Key Decisions Framework

```
Synchronous vs Async?
  → Strong consistency needed + simple flow → Sync (gRPC/REST)
  → Decoupling, resilience, events → Async (Kafka/MQ)

Choreography vs Orchestration?
  → Simple, independent steps → Choreography
  → Complex workflow, rollback needed → Orchestration (Saga)

Where to put shared logic?
  → Cross-cutting (auth, logging) → Sidecar / Service Mesh
  → Business logic → NEVER share; duplicate or move to domain service

How to handle failure?
  → Timeout + Retry + Circuit Breaker + Fallback (in that order)
```

## "What would you do if..." Patterns

| Scenario | Answer |
|---|---|
| Service A needs data from B synchronously but B is slow | Circuit breaker + fallback to cache |
| Two services need atomicity across their DBs | Saga (orchestrated or choreographed) |
| Event fired twice (at-least-once) | Idempotency key + deduplication |
| Need to add new field to API without breaking clients | Additive change + Tolerant Reader pattern |
| Monolith is growing too big | Strangler Fig, identify bounded contexts, extract services |
| Service response time is p99 = 5s | Distributed trace to find bottleneck, check tail latency, hedged requests |
| Cascading failure in prod | Circuit breakers, bulkheads — check if tripped. Kill unhealthy services. Shed load. |
| How to deploy without downtime | Blue-Green or Canary with automated rollback |
| Secrets in environment variables | Move to Vault / Secrets Manager, use K8s External Secrets |

---

*Notes prepared for senior microservices architect interview — covers fundamentals through advanced patterns, performance, observability, and technology ecosystem.*
