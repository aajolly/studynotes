# Requirements gathering, analysis, and design
Requirements gathering, analysis, and design—a foundational topic for cloud architects. This includes techniques, best practices, and tools to ensure clarity and alignment with stakeholders.

## Qualitative Requirements
- Qualitative requirements define systems from the user's point of view
- These five questions help define the scope and context of a system:
| **Question** | **Purpose** | **Examples** |
| :--------- | :------- | :------- |
| **Who** | Identify users, developers, stakeholders—anyone affected by the system. | Who are the users? Who are the the developers? Who are the stakeholders |
| **What** | Define the system’s functionality in clear, unambiguous terms. | What does the system do? What are the main features? |
| **Why** | Understand the problem being solved, helps define KPIs, SLOs, and SLAs. | Why is the system needed? |
| **When** | Establish timelines and scope boundaries. | WHhen do the users need and/or want the solution? When can the developers be done? |
| **How** | Determine non-functional requirements (e.g., performance, concurrency, latency). | How will the system work? How many users will there be? How much data will there be? |

## User Roles and Personas
- **Role**
    - Roles are not poeple or job titles
        - People can play multiple roles
        - A single role can be played by multiple people
    - Roles should decscrive a users objective
        - What does the user want to do?
        - "User" is not a good role (everyone is a user)
    - Examples of roles:
        - Shopper
        - Account Holder
        - Customer
        - Administrator
        - Manager
- **Persona**
    - Personas decscibe a typical person who plays a role
        - Personas tell a story of the person is
        - Peronas are **not** a list of job functions
        - For each role, there could be many personas
    - **Example**: Alice is a busy working mom who wants to access ABC Bank to check her account balances and make sure that there are enough funds to pay for her kids' music and sport lessons. She also uses the website to automate payment of bills and view here credit account balances. Alice wants to save time and money, and she wants a credit card that gives her cash back.

### Role Identification Process
- **Brainstorm**: List all possible roles.
- **Organize**: Group overlapping or related roles.
- **Consolidate**: Remove duplicates and merge similar roles.
- **Refine**: Add details like expertise level, usage frequency, internal vs. external.

## User Stories
User stories describe a feature from the user's point of view:
- Give each story a title that describes its purpose.
- Write a short, one sentence description.
- Specify the user role, what they want to do, and why.
- Use a format like below:
    - **Format 1**: "As a [type of user], I want to [do something] so that I can [achieve a benefit]."
    - **Format 2**: "Given [context], when [action], then [expected outcome]."
**Example**:
- **Title**: Balance Inquiry
- **Story**: "As an account holder, I want to check my available balance at any time of day so I am sure not to overdraw my account."

### INVEST Criteria for Good User Stories
| **Letter** | **Meaning** | **Description** |
| I | Independent | Can be developed and delivered separately. |
| N | Negotiable | Encourages collaboration and refinement. |
| V	| Valuable | Delivers value to the user. |
| E	| Estimatable | Can be estimated for planning. |
| S	| Small | Narrow in scope for quick feedback. |
| T	| Testable | Can be verified through testing. |

# Quantitative Requirements
Quantitative requirements are things that are measurable
Given the constraints:
- Time
- Finance
- People
What can be achieved:
- How many users are there?
- How much data is there?
- What are the rewards and risks?
- Which features can launched?
## KPIs
Key Performance Indicators (KPIs) are metrics that can be used to measure success
In business, common KPIs include:
- Return on Investment (ROI)
- Earnings before interest and taxes (EBIT)
- Employee turnover
- Customer churn
In software, common KPIs include:
- Page views
- User registrations
- Clickthroughs
- Checkouts
### KPIs vs Goals
- **Goal**: The desired outcome (e.g., increase online store revenue).
- **KPI**: A measurable indicator of progress toward that goal (e.g., conversion rate).
- **Best Practice**:
    - Start with a goal → define KPIs → set targets → monitor progress → adjust as needed.

#### SMART KPIs (INVEST-aligned)
| **Attribute** | **Description** |
| :--------- | :--------- |
| **Specific** | Clearly defined (e.g., “Section 508 accessible” vs. “user-friendly”). |
| **Measurable** | Quantifiable, you have to find any objective way to test whether you're meeting your KPIs (e.g., “<400ms response time”). |
| **Achievable** | Realistic (e.g., not 100% availability). |
| **Relevant** | Aligned with business goals. |
| **Time-bound** | Defined time frame (e.g., 99% available: per day? per month? per year? If we don't know, how can be measure). |

### Service Level Terminology
| **Term** | **Definition** | **Example** |
| :------ | :--------- | :-------- |
| **SLI (Service Level Indicator)** | A measurable aspect of service (e.g., latency, error rate).	| “HTTP GET requests < 400ms” |
| **SLO (Service Level Objective)**	| Target value or range for an SLI.	| “Latency < 100ms for 99% of requests” |
| **SLA (Service Level Agreement)**	| Formal agreement with penalties if unmet.	| “99.9% uptime per month” |

> Architect for SLOs, not just SLAs, to maintain buffer and reliability.

#### Aggregation & Percentiles
- Averages can be misleading: e.g., 500 RPS average may hide 1000 RPS bursts.
- Use percentiles for latency and performance:
    - 50th percentile (P50): Typical case
    - 99th percentile (P99): Worst-case experience

## SLOs, SLIs, and SLAs
![slo](/images/gcp/slo.png)
### Tips for determining SLOs
- The goal isn't to make SLOs as high as possible, the goal is to make them as low as you can get away with while still making users happy. That's why it's important to understand your users
- The higher you set the SLO, the higher the cost in compute resources (redundancy) and operations effort (poeple time).
- Applications should not significantly outperform their SLOs, because users come to expect the level of reliability you usually give them.

### SLAs
An SLA is a business contract between the provider and the customer
The SLA stipulates that:
- A penalty will apply to the provider if the service does not maintain certain availability and/or performance thresholds.
- If the SLA is broken, the customer will receive compensation from the provider.
Not all services have an SLA, but all services should have an SLO.
Your SLO thresholds should be stricter than your SLA.
![example-sli-slo-sla](/images/gcp/example-sli-slo-sla.png)
Example SLO/SLI for a travel portal application
![example-app-slo-sli](/images/gcp/example-app-sli-slo.png)

# Microservices
![microservices](/images/gcp/microservices.png)

## Monolithvs Microservices
| **Monolithic Architecture** | **Microservices Architecture** |
| :------- | :-------- |
| Single codebase and database | Multiple independent services |
| Deployed as a single unit | Independently deployable units |
| Tightly coupled components | Loosely coupled, modular components |
| Harder to scale selectively | Services can scale independently |

## Why Use Microservices?
- **Team Autonomy**: Teams can develop, deploy, and scale independently.
- **Faster Delivery**: Parallel development and deployment cycles.
- **Scalability**: Scale only the services that need it.
- **Resilience**: Failures are isolated to individual services.
- **Cost Accounting**: Easier to track resource usage per service.

## Design Principles
- Each service should have its own datastore to avoid tight coupling.
- Define strong service contracts (APIs).
- Enable independent deployments and rollbacks.
- Support A/B testing and canary releases.
- Improve observability with isolated logging and monitoring.

## Challenges of Microservices
- **Boundary Definition**: Hard to define clear service boundaries.
- **Infrastructure Complexity**: More moving parts and failure points.
- **Latency & Resilience**: Network calls introduce delays and require retries.
- **Security**: Service-to-service communication must be secured.
- **Versioning**: APIs must maintain backward compatibility.

## Decomposition Strategy
- Use Domain-Driven Design (DDD) to identify logical groupings.
- Example: For an e-commerce app, group by:
    - Product Management
    - Reviews
    - Accounts
    - Orders
- Identify shared services (e.g., authentication) and isolate them.

## State Management
- Stateless services are easier to scale and manage.
- Stateful services are sometimes necessary but introduce complexity.
### Best Practices for Stateful Services
| **Challenge** | **Solution** |
| :--------- | :-------- |
| In-memory state | Avoid; requires sticky sessions (session affinity). |
| Persistent state | Use managed services like Cloud SQL, Firestore. |
| Caching | Use Memorystore for Redis for fast access. |

![gcp-example-microservices](/images/gcp/example-microservices.png)

## Best Practices
The 12-factor app is a set of best practices for building web or software-as-a-service applications
![12-factor-app-part1](/images/gcp/12-factor-app-part1.png)
![12-factor-app-part2](/images/gcp/12-factor-app-part2.png)
![12-factor-app-part3](/images/gcp/12-factor-app-part3.png)

# REST and APIs
## Core Principle: Loose Coupling via Strong Contracts
- **Independence**: Microservices must be independently deployable.
- **Versioned Contracts**: Each service must expose a versioned API and maintain backward compatibility.
- **Deprecation Policy**: Ensure older versions remain available until no clients depend on them (e.g., for rollback scenarios).

## Communication Protocols
| **Protocol** | **Use Case** | **Format** | **Notes** |
| :--------- | :--------- | :-------- | :------- |
| HTTP/REST | Public APIs, general use | JSON (or XML) | Widely adopted, human-readable |
| gRPC | Internal services, high performance | Protocol Buffers | Supports streaming, compact and fast |

## RESTful Design Principles
- Resources: Identified by URIs (e.g., /dogs/123).
- Representations: JSON or XML payloads representing the resource (e.g., { "name": "Noir", "breed": "Schnoodle" }).
- HTTP Verbs:
    - GET: Retrieve
    - POST: Create
    - PUT: Update
    - DELETE: Remove
- Batch APIs: Return collections for performance (e.g., /dogs returns all dogs).
- Hypermedia (HATEOAS): Responses include links to related resources, reducing client-side assumptions.

## API Design Best Practices
- **Use OpenAPI** (Swagger) for REST; Protocol Buffers for gRPC.
- **Design around domains**, not specific clients or use cases.
- **Consistent URI structure**, error handling, and pagination.
- **Caching**: Use for immutable resources to improve performance.
- **Minimal Client Knowledge**: Clients should only need URI, request format, and response format.

## Security & Compatibility
- **HTTPS**: Always use secure communication.
- **Backward Compatibility**: Avoid breaking changes; version APIs (e.g., /v1/orders).
- **Interface Management**: Track and version service interfaces rigorously.

## Common HTTP Verbs in REST
| **Verb** | **Purpose** | **Idempotent?** |
| :-------- | :--------- | :---------- |
| GET | Retrieve a resource	| Yes |
| POST | Create a new resource | No |
| PUT | Create or update a resource | Yes |
| DELETE | Remove a resource | Yes |

## Common HTTP Status Codes
| **Code** | **Meaning** |
| :----- | :------ |
| 200 | OK |
| 201 | Created |
| 400 | Bad Request |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

## REST API Design Best Practices (HTTP Lens)
- Use singular nouns for individual resources (/pet/1)
- Use plural nouns for collections (/pets)
- Avoid verbs in URIs (e.g., use GET /pets, not GET /getpets)
- Include versioning in URIs (e.g., /v1/pets)
- Ensure consistent error handling, URI structure, and pagination
- Use batch APIs for performance (e.g., return multiple items in one call)
- Consider caching for immutable resources

## API design in Google Cloud
### API Design Principles
- Consistency is key: Naming, error handling, versioning, and documentation should follow a unified style.
- Google API Design Guide: Offers detailed recommendations and is used across Google Cloud services.
- Design-first approach: Use OpenAPI (formerly Swagger) to define APIs before implementation.

### Google Cloud API Structure
- Format: service.collection.verb
- Service: API endpoint (e.g., https://compute.googleapis.com)
- Collection: Resource group (e.g., instances, instanceGroups)
- Verb: Action (e.g., GET, LIST, INSERT)
- Example: To list Compute Engine instances: GET https://compute.googleapis.com/compute/v1/projects/{project}/zones/{zone}/instances

### OpenAPI (Swagger)
- OpenAPI: Industry standard for REST API definitions.
- Swagger: Tooling ecosystem around OpenAPI.
- Benefits:
    - Single source of truth
    - Auto-generates client libraries and server stubs
    - Generates interactive documentation
- Supported by: Cloud Endpoints, Apigee

### gRPC for Internal APIs
- gRPC: High-performance, binary protocol based on HTTP/2.
- Protocol Buffers: Define service contracts.
- Supports:
    - Multiple languages
    - Streaming (client/server/bidirectional)
    - Strong typing and backward compatibility
- Used with: GKE, Cloud Endpoints, Application Load Balancer (via Envoy)

### API Management Tools in Google Cloud
| **Tool** | **Description** |
| :------ | :---------- |
| **Cloud Endpoints** | Lightweight API gateway for GCP backends. Supports OpenAPI and gRPC. |
| **Apigee** | Enterprise-grade API management platform. Supports hybrid and multi-cloud. Includes analytics monetization, and developer portals. |
| **API Gateway** | Secure, scalable gateway for REST APIs. Works across GCP services. |

> All three tools support: Authentication, Monitoring, Rate limiting, OpenAPI and gRPC

# Key storage characteristics in Google Cloud
## Types of Google Cloud Storage Services
| **Category** | **Services** |
| :------ | :---------- |
| Relational | Cloud SQL, Cloud Spanner |
| NoSQL | Firestore, Bigtable |
| Object Storage | Cloud Storage |
| Data Warehouse | BigQuery |
| In-Memory | Memorystore (Redis, Memcached) |

## Key Storage Characteristics to Consider
| **Characteristic** | **Description** |
| :------ | :---------- |
| **Data Type** | Structured (SQL), semi-structured (JSON), unstructured (images, logs) |
| **Scale** | Vertical (Cloud SQL, Memorystore) vs Horizontal (Spanner, Bigtable) |
| **Durability** | Likelihood of data loss (e.g., Cloud Storage offers 11 9’s durability) |
| **Availability** | Varies by configuration (e.g., multi-region > regional > zonal) |
| **Consistency** | Strong (Cloud SQL, Spanner, Firestore) vs Eventual (Bigtable, Memorystore) |
| **Cost Efficiency** | Depends on storage size, access frequency, and read/write patterns |
| **Location** | Regional vs multi-regional deployments impact latency and availability |

## Consistency Models
| **Service** | **Consistency Type** |
| :------ | :---------- |
| Cloud SQL | Strong |
| Spanner | Strong |
| Firestore | Strong |
| Cloud Storage | Strong |
| Bigtable | Strong (single cluster) |
| Memorystore | Eventual |

## Cost Considerations
| **Service** | **Notes** |
| :------ | :---------- |
| **Bigtable/Spanner** | High throughput, not cost-efficient for small datasets |
| **Firestore** | Low storage cost, but read/write operations can add up |
| **Cloud Storage** | Low cost, ideal for static or infrequently accessed data |
| **BigQuery** | Cheap storage, but query costs can be high |

## Data Transfer Options
| **Method** | **Use Case** |
| :------ | :---------- |
| Storage Transfer Service | Online transfer from cloud/on-prem to GCS |
| Transfer Appliance | Offline transfer for very large datasets |
| BigQuery Data Transfer Service | Scheduled ingestion from SaaS or cloud sources |

## Decision-Making Tips
- Use **Cloud Storage** for static files, backups, and media.
- Use **Cloud SQL** for traditional relational workloads.
- Use **Spanner** for globally distributed, strongly consistent relational data.
- Use **Firestore** for mobile/web apps needing real-time updates.
- Use **Bigtable** for high-throughput, low-latency NoSQL workloads.
- Use **BigQuery** for analytical queries over large datasets.
- Use **Memorystore** for caching and session management.
![gcp-storage-services](/images/gcp/gcp-storage-services.png)
![gcp-storage-services-decision-tree](/images/gcp/gcp-storage-services-decision-tree.png)

# Google Cloud Deployment Platforms
![gcp_choose_deployment_option](/images/gcp/gcp_choose_deployment_pattern.png)

## Google Kubernetes Engine (GKE)
**What it is**: A managed Kubernetes service for deploying, managing, and scaling containerized applications.
**Key Concepts**:
- Uses Compute Engine VMs to form clusters.
- Kubernetes handles orchestration: deployments, scaling, monitoring, and policies.
- Pods are the smallest deployable units, containing one or more containers.
- Two modes:
    - **Autopilot**: Fully managed by Google (recommended).
    - **Standard**: More control and flexibility.

> **AWS Equivalent**: Amazon EKS (Elastic Kubernetes Service)

## Cloud Run
**What it is**: A fully managed platform to run containers without managing servers.
**Deployment Options**:
- From Artifact Registry (by tag or digest).
- Directly from source code (auto-containerized and stored in Artifact Registry).
**Revisions**:
- Immutable.
- Each deployment creates a new revision.

**Tools**: Deploy via Google Cloud Console or gcloud CLI.

> **AWS Equivalent**: AWS App Runner or AWS Fargate (for serverless containers)

## App Engine
**What it is**: A serverless platform for building and deploying applications.
**Features**:
- Auto-scaling from zero.
- Designed for microservices.
- Supports traffic splitting (e.g., A/B testing, canary deployments).
- Structure:
    - One App Engine app per project.
    - App → Services → Versions → Instances.
- Architecture Example:
    - Frontend: App Engine.
    - Backend:
        - Cloud Storage (static content)
        - Cloud SQL (relational data)
        - Firestore (NoSQL)
        - Memcache (caching)
        - Cloud Tasks (async processing)

> **AWS Equivalent**: AWS Elastic Beanstalk (for managed app deployment), AWS Lambda (for serverless), Amazon S3, Amazon RDS, Amazon DynamoDB, Amazon ElastiCache, Amazon SQS

## Cloud Run Functions
**What it is**: Event-driven, loosely coupled microservices.
**Triggers**:
- Cloud Storage events
- Pub/Sub messages
- HTTP requests
**Billing**: Based on execution time (100ms increments); no cost when idle.
**Example Use Case**:
- Image uploaded → OCR function (Vision API) → Pub/Sub → Translation function → Pub/Sub → File write function.

> **AWS Equivalent**: AWS Lambda with Amazon S3, Amazon SNS/SQS, Amazon Translate, Amazon Rekognition

