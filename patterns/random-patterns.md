### Service Design Patterns
- **Service Contract Design**: Defines the interface, data formats, and protocols for service interaction.  
  *Example: A RESTful API specification using OpenAPI.*
- **Service Façade**: Provides a simplified interface to a complex service or subsystem.  
  *Example: A `PaymentFacade` that combines payment processing, fraud check, and logging.*
- **Service Wrapper**: Adapts a legacy system or external API to fit a standardized service interface.  
  *Example: Wrapping a legacy SOAP service with a RESTful API.*
- **Service Normalization**: Transforms disparate data formats into a common canonical format.  
  *Example: Converting XML, JSON, and CSV inputs to a unified JSON structure.*
- **Service Decomposition**: Breaking a large system into smaller, focused services.  
  *Example: Splitting a monolithic e-commerce app into Order, Inventory, and Customer services.*
- **Capability Composition**: Combining multiple service capabilities to create a new, higher-level service.  
  *Example: A `CheckoutService` that composes Payment, Shipping, and Inventory services.*
- **Agnostic Service**: A service designed to be reusable across multiple business contexts.  
  *Example: A `NotificationService` that sends emails/SMS for any business domain.*
- **Non-Agnostic Service**: A service tightly coupled to a specific business process.  
  *Example: A `CheckoutService` that is specific to the e-commerce checkout flow.*
- **Utility Service**: Provides common, cross-cutting functionality like logging or authentication.  
  *Example: An `AuthService` handling user authentication for all applications.*
- **Entity Service**: Manages CRUD operations for a business entity (e.g., Customer, Order).  
  *Example: `CustomerService` with methods like `getCustomer`, `updateCustomer`.*
- **Task Service**: Orchestrates a business process by coordinating multiple services.  
  *Example: `OrderFulfillmentTask` that calls Inventory, Payment, and Shipping services.*

### Service Interaction Patterns
- **Service Composition**: Combining multiple services to achieve a business goal.  
  *Example: An online travel agency composes Flight, Hotel, and Car rental services.*
- **Orchestration**: Central coordinator (orchestrator) controls the flow of service invocations.  
  *Example: An `OrderOrchestrator` sequentially calls Payment, Inventory, and Shipping.*
- **Choreography**: Services collaborate through events without a central controller.  
  *Example: When order is placed, an event triggers Inventory to reserve stock, then Shipping to prepare delivery.*
- **Asynchronous Messaging**: Services communicate via messages without blocking.  
  *Example: Order service sends a message to a queue, and Inventory service processes it later.*
- **Competing Consumers**: Multiple instances of a consumer compete to process messages from a queue.  
  *Example: Several worker instances pulling tasks from a work queue for parallel processing.*
- **Event-Driven Messaging**: Services react to events published asynchronously.  
  *Example: A "OrderPlaced" event triggers notifications, inventory updates, and analytics.*
- **Callback**: A service provides a callback endpoint for asynchronous result delivery.  
  *Example: A payment service calls back to a `paymentWebhook` URL when transaction completes.*
- **Service Router**: Routes requests to appropriate service endpoints based on rules.  
  *Example: An API gateway routing `/users` to UserService and `/orders` to OrderService.*

### Service Governance Patterns
- **Canonical Data Model**: A common data format used across services to reduce translation.  
  *Example: All services exchange customer data using a standard `Customer` JSON schema.*
- **Contract Centralization**: Storing service contracts in a central repository for governance.  
  *Example: All OpenAPI specifications are stored in a shared Git repository.*
- **Service Registry**: A central directory where services register their endpoints and metadata.  
  *Example: Netflix Eureka where each microservice registers its IP and port.*
- **Policy Centralization**: Central management of policies like security, throttling, and logging.  
  *Example: A central policy engine that enforces rate limits across all APIs.*
- **SLA Centralization**: Monitoring and reporting service-level agreements from a central dashboard.  
  *Example: A central tool tracking uptime and response times for all services.*

### MICROSERVICES Architectural Patterns
- **Bounded Context**: Defines explicit boundaries for a domain model within a microservice.  
  *Example: In an e-commerce system, "Inventory" and "Order" are separate bounded contexts.*
- **API Gateway**: A single entry point for clients that routes requests to appropriate services.  
  *Example: Kong or AWS API Gateway routing requests to microservices.*
- **Database per Service**: Each microservice has its own private database.  
  *Example: Order service uses a PostgreSQL database, while Inventory service uses MongoDB.*
- **Decentralized Governance**: Teams choose technologies best suited for their service.  
  *Example: One service written in Python, another in Go, communicating via HTTP.*
- **Backend for Frontend (BFF)**: Dedicated backend per frontend client (e.g., mobile, web).  
  *Example: A mobile BFF that aggregates data specifically for mobile app needs.*
- **Service Registry**: As defined above, a registry for service discovery.  
  *Example: Consul or etcd used by microservices to find each other.*
- **Service Discovery**: Clients dynamically find service locations via a registry.  
  *Example: Using client-side discovery with Netflix Ribbon to locate service instances.*

### MICROSERVICES Communication Patterns
- **Synchronous REST**: Services communicate directly via HTTP REST calls.  
  *Example: Order service calls `GET /inventory/stock` to check availability.*
- **Asynchronous Messaging**: Services communicate via message brokers (RabbitMQ, Kafka).  
  *Example: Order service publishes an `OrderCreated` event to Kafka.*
- **Event Streaming**: Continuous flow of events consumed by multiple services.  
  *Example: Apache Kafka streams all customer actions for real-time analytics.*
- **Pub/Sub**: Publishers send messages to topics, and subscribers receive them.  
  *Example: Google Pub/Sub where inventory updates are broadcast to subscribers.*

### MICROSERVICES Data Patterns
- **CQRS** (Command Query Responsibility Segregation): Separates write and read models.  
  *Example: Commands update a write database, queries read from a denormalized read replica.*
- **Event Sourcing**: Stores state changes as a sequence of events.  
  *Example: Instead of storing current balance, store all "AccountCredited" and "AccountDebited" events.*
- **Saga**: Manages distributed transactions across services using compensating actions.  
  *Example: If payment fails after order creation, a compensating action cancels the order.*
- **API Composition**: A service aggregates data by calling multiple services.  
  *Example: A `CustomerProfile` service calls `CustomerDetails` and `OrderHistory` services.*
- **Aggregator**: Similar to API composition but often implemented as a separate service.  
  *Example: A `DashboardService` aggregates metrics from multiple microservices.*

### MICROSERVICES Resiliency Patterns
- **Circuit Breaker**: Prevents repeated calls to a failing service.  
  *Example: Hystrix opens circuit after 5 failures, then requests fail fast.*
- **Retry**: Automatically retries failed operations with backoff.  
  *Example: A network call retries up to 3 times with exponential backoff.*
- **Timeout**: Limits wait time for a response to avoid hanging.  
  *Example: Setting HTTP client timeout to 5 seconds.*
- **Bulkhead**: Isolates resources to prevent failure in one component from cascading.  
  *Example: Separate thread pools for different service clients.*
- **Idempotent Receiver**: Ensures processing the same request multiple times has the same effect.  
  *Example: A payment API that uses idempotency keys to avoid duplicate charges.*

### Cloud Architecture Patterns
- **Elastic Resource Capacity**: Dynamically allocate resources based on demand.  
  *Example: AWS Auto Scaling adds EC2 instances during peak traffic.*
- **Resource Pooling**: Sharing resources among multiple users.  
  *Example: Multi-tenant database where each tenant's data is logically separated.*
- **Resource Replication**: Duplicating resources for availability and performance.  
  *Example: Database read replicas to handle read-heavy workloads.*
- **Dynamic Scalability**: Automatically scale resources up or down.  
  *Example: Kubernetes Horizontal Pod Autoscaler based on CPU usage.*
- **Failover System**: Automatically switch to a standby system on failure.  
  *Example: Primary database fails, traffic routes to a replica.*
- **Load Balanced Virtual Server**: Distributes traffic across multiple server instances.  
  *Example: AWS ELB distributing requests to multiple EC2 instances.*
- **Redundant Storage**: Data stored across multiple devices or locations.  
  *Example: RAID, or cloud storage with geo-redundancy.*
- **Audit Monitor**: Tracks and logs system activities for compliance.  
  *Example: AWS CloudTrail logging all API calls.*
- **Usage Monitor**: Measures resource consumption for billing or analysis.  
  *Example: Monitoring CPU, memory usage per tenant.*
- **Pay-Per-Use Monitor**: Tracks usage for metered billing.  
  *Example: Cloud provider tracking API calls to charge accordingly.*

### Cloud Security Patterns
- **Identity Federation**: Allowing users to authenticate via external identity providers.  
  *Example: Using Google or Facebook login for an application.*
- **Encrypted Storage**: Data encrypted at rest.  
  *Example: AWS EBS volumes encrypted with KMS.*
- **Secure Communication**: Data encrypted in transit (TLS).  
  *Example: HTTPS for all service endpoints.*
- **Multi-Tenancy Isolation**: Logical separation of tenant data and resources.  
  *Example: Kubernetes namespaces to isolate different teams.*

### Cloud Operational Patterns
- **Automated Scaling Listener**: Monitors metrics and triggers scaling actions.  
  *Example: CloudWatch alarm triggers Auto Scaling.*
- **Resource Reservation**: Pre-reserving resources for guaranteed capacity.  
  *Example: AWS Reserved Instances for cost savings.*
- **Cloud Balancing**: Distributing workloads across multiple clouds or regions.  
  *Example: Using a global load balancer to route traffic to multiple cloud providers.*

### BigData Architecture Patterns
- **Data Lake**: Stores raw data in native format until needed.  
  *Example: Amazon S3 used as a data lake for structured and unstructured data.*
- **Data Warehouse**: Stores structured, processed data for analytics.  
  *Example: Amazon Redshift for business intelligence queries.*
- **Data Lakehouse**: Combines data lake flexibility with warehouse management.  
  *Example: Databricks Delta Lake enabling ACID transactions on data lake.*
- **Data Mesh**: Decentralized data architecture with domain-oriented ownership.  
  *Example: Each business domain (sales, marketing) owns its data as a product.*
- **Schema-on-Read**: Data structure applied when reading, not writing.  
  *Example: Reading CSV files with varying schemas in Apache Spark.*
- **Schema-on-Write**: Data validated and structured before storage.  
  *Example: Traditional RDBMS where table schema is defined before inserting.*

### BigData Processing Patterns
- **Batch Processing**: Processing large volumes of data at scheduled intervals.  
  *Example: Hadoop MapReduce job running nightly.*
- **Stream Processing**: Processing data in real-time as it arrives.  
  *Example: Apache Flink analyzing clickstream data continuously.*
- **Micro-Batch Processing**: Small batches processed at short intervals.  
  *Example: Spark Streaming with 5-second micro-batches.*
- **Real-Time Analytics**: Immediate insights from streaming data.  
  *Example: Dashboards showing live website traffic.*

### BigData Governance Patterns
- **Data Lineage**: Tracking data origins, transformations, and movement.  
  *Example: Apache Atlas capturing metadata of data flows.*
- **Metadata Centralization**: Central repository for metadata management.  
  *Example: A data catalog like Amundsen for discovering datasets.*
- **Master Data Management**: Ensuring consistency of core business entities.  
  *Example: Centralized customer data management across systems.*
- **Data Quality Monitoring**: Continuously checking data for accuracy and completeness.  
  *Example: Great Expectations validating data pipelines.*

### DevOps Pipeline Patterns
- **Continuous Integration**: Automatically build and test code on every commit.  
  *Example: GitHub Actions running tests on each push.*
- **Continuous Delivery**: Automatically deploy to staging after CI, ready for manual release.  
  *Example: Jenkins pipeline deploying to staging, then requiring approval for production.*
- **Continuous Deployment**: Automatically deploy to production after passing tests.  
  *Example: Every merged PR triggers a deployment to production.*
- **Pipeline as Code**: Defining CI/CD pipelines in version-controlled files.  
  *Example: Jenkinsfile or GitLab CI YAML.*

### DevOps Infrastructure Patterns
- **Infrastructure as Code**: Managing infrastructure through machine-readable definition files.  
  *Example: Terraform scripts to provision AWS resources.*
- **Immutable Infrastructure**: Replacing rather than modifying servers.  
  *Example: Deploying a new AMI instead of patching an existing instance.*
- **Configuration as Code**: Managing configuration files in version control.  
  *Example: Ansible playbooks to configure servers.*
- **Environment Replication**: Ensuring dev, test, and prod environments are identical.  
  *Example: Using Docker containers to replicate production locally.*

### DevOps Release Patterns
- **Blue-Green Deployment**: Two identical environments; switch traffic after new version is ready.  
  *Example: Blue is live, deploy to Green, then route traffic to Green.*
- **Canary Release**: Rolling out new version to a small subset of users first.  
  *Example: 5% of traffic to new version, monitor, then gradually increase.*
- **Rolling Deployment**: Gradually updating instances one by one.  
  *Example: Kubernetes rolling update replacing pods incrementally.*

### DevOps Observability Patterns
- **Centralized Logging**: Aggregating logs from all services into a single system.  
  *Example: ELK stack (Elasticsearch, Logstash, Kibana).*
- **Distributed Tracing**: Tracking requests across multiple services.  
  *Example: Jaeger or Zipkin for tracing microservices.*
- **Health Monitoring**: Regularly checking service health and alerting on failures.  
  *Example: Prometheus scraping `/health` endpoints and alerting.*
- **Error Budget Governance**: Using error budgets to balance reliability and feature velocity.  
  *Example: SLO of 99.9% uptime allows 0.1% error budget; if exceeded, features freeze.*

### SECURITY PATTERNS (Cross-Domain)
- **Zero Trust Model**: Never trust, always verify; no implicit trust based on network location.  
  *Example: Every request authenticated and authorized regardless of source.*
- **RBAC** (Role-Based Access Control): Access based on user roles.  
  *Example: Admin, Editor, Viewer roles with different permissions.*
- **Token-Based Authentication**: Using tokens (JWT) for stateless authentication.  
  *Example: Client sends JWT in Authorization header.*
- **OAuth2**: Authorization framework allowing third-party access without sharing credentials.  
  *Example: "Login with Google" using OAuth2.*
- **API Security Gateway**: Central gateway handling authentication, rate limiting, etc.  
  *Example: Kong or AWS API Gateway with security plugins.*
- **Secrets Management**: Secure storage and access of secrets (passwords, keys).  
  *Example: HashiCorp Vault or AWS Secrets Manager.*
- **Encryption at Rest**: Data stored encrypted.  
  *Example: Database encryption using TDE.*
- **Encryption in Transit**: Data encrypted during transmission.  
  *Example: TLS for all network communication.*

### GOVERNANCE & ENTERPRISE PATTERNS
- **Architecture Review Board**: Group that reviews and approves architectural decisions.  
  *Example: Monthly meeting to assess new technology adoption.*
- **Policy Enforcement**: Automated enforcement of organizational policies (security, compliance).  
  *Example: Open Policy Agent (OPA) enforcing policies in Kubernetes.*
- **Centralized Monitoring**: Single platform for monitoring all services and infrastructure.  
  *Example: Datadog or Prometheus/Grafana stack.*
- **Compliance Automation**: Automating compliance checks (e.g., SOC2, HIPAA).  
  *Example: Automated scans for PCI DSS compliance.*
- **Risk Mitigation Framework**: Structured approach to identify and reduce risks.  
  *Example: Threat modeling and regular security assessments.*
- **Service Portfolio Management**: Managing the lifecycle of all services.  
  *Example: Cataloging all services with owners, versions, and status.*
- **Platform Engineering Model**: Building internal platforms to enable self-service for development teams.  
  *Example: A developer portal with predefined infrastructure templates.*

### Arcitura Patterns
(Note: These are often covered in SOA training; some overlap with above.)
- **Service Loose Coupling**: Minimizing dependencies between services.  
  *Example: Services communicate via contracts without sharing internal logic.*
- **Service Autonomy**: Services have control over their own environment.  
  *Example: Each service runs in its own process/container.*
- **Standardized Service Contract**: Services adhere to a common contract design.  
  *Example: All services expose RESTful APIs with consistent error handling.*
- **Service Abstraction**: Hiding internal details from consumers.  
  *Example: Only the service interface is exposed; implementation is hidden.*
- **Elastic Resource Capacity**: As described earlier (cloud pattern).  
  *Example: Auto-scaling based on demand.*
- **Failover System**: As described earlier.  
  *Example: Automatic failover to standby instance.*
- **Resource Replication**: As described earlier.  
  *Example: Database replication.*
- **CQRS**: As described earlier.  
  *Example: Separate read/write models.*
- **Saga**: As described earlier.  
  *Example: Distributed transaction management.*
- **Event-Driven Messaging**: As described earlier.  
  *Example: Services react to events.*
- **Database per Service**: As described earlier.  
  *Example: Each microservice owns its database.*
- **Canonical Data Model**: As described earlier.  
  *Example: Standardized data formats.*