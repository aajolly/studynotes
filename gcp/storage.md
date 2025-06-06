# Google Cloud Storage and Database Options
![gcp-storage-db-options](/images/gcp/gcp-storage-db-options.png)
![gcp-storage-db-decision-chart](/images/gcp/gcp-storage-db-decision-chart.png)
# Google Cloud Storage
**Cloud Storage** is Google Cloud’s **object storage** service designed for durability, scalability, and high availability. It’s used to store and retrieve **binary large objects (BLOBs)** such as images, videos, backups, and large datasets.

## Object Storage Basics
- **Object** = Data + Metadata + Unique Identifier (URL-based)
- Unlike file or block storage, object storage doesn’t use a folder hierarchy or disk blocks.
- Common use cases: media content, backups, archives, and intermediate data in workflows.
> **AWS Equivalent**: Amazon S3

## Buckets
- Buckets are containers for objects.
- Each bucket has:
    - A **globally unique name**
    - A **geographic location** (region, multi-region, or dual-region)
- Choose a location close to your users to reduce latency.

## Object Immutability & Versioning
- Objects are **immutable**, you can’t edit them, only replace them.
- **Versioning** (optional):
    - Keeps a history of object changes (overwrites/deletes).
    - Allows restoring or deleting specific versions.
    - If disabled, new uploads overwrite existing objects.

## Access Control
- **IAM Roles** (recommended):
    - Permissions inherited from project → bucket → object.
    - Follows least-privilege principle.
- **Access Control Lists (ACLs)** (for fine-grained control):
    - Define **scope** (who) and **permission** (what: read/write).
    - Useful for specific user-level access.
> **AWS Equivalent**: IAM policies and S3 bucket policies

## Lifecycle Management
- Automate storage cost optimization:
    - Delete objects older than a set number of days.
    - Remove objects created before a specific date.
    - Retain only the most recent versions (if versioning is enabled).
- Helps manage storage costs and enforce data retention policies.
> **AWS Equivalent**: S3 Lifecycle Policies

## Common Use Cases
- Hosting static website content
- Serving media files (images, videos)
- Backup and disaster recovery
- Data lake storage for analytics
- Intermediate storage for data pipelines

## Encryption Options
- **Google Cloud**: Supports Customer-Supplied Encryption Keys (CSEK) for Cloud Storage.
- **AWS Equivalent**: AWS S3 supports similar functionality via Server-Side Encryption with Customer-Provided Keys (SSE-C) and AWS Key Management Service (SSE-KMS).

## Object Lifecycle Management
- **Google Cloud**: Automates actions like deleting, archiving, or changing storage class based on object age or version count.
- **AWS Equivalent**: Amazon S3 Lifecycle Policies allow similar rules for transitioning objects between storage classes (e.g., S3 Standard to Glacier) or deleting them after a set period.

## Object Versioning
- **Google Cloud**: Maintains multiple versions of objects; each version is uniquely identified by a generation number.
- **AWS Equivalent**: S3 Versioning allows you to preserve, retrieve, and restore every version of every object stored in a bucket.

## Soft Delete
- **Google Cloud**: Retains deleted or overwritten objects for a configurable retention period (default 7 days).
- **AWS Equivalent**: S3 Object Lock with a "Governance" or "Compliance" mode can prevent object deletion for a specified period. AWS also supports MFA Delete for added protection.

## Object Immutability
- **Google Cloud**: Objects are immutable; overwriting creates a new version.
- **AWS Equivalent**: S3 also treats objects as immutable unless overwritten or deleted, and Object Lock can enforce immutability.

## Directory Synchronization
- **Google Cloud**: Syncs VM directories with buckets.
- **AWS Equivalent**: AWS CLI and AWS DataSync can be used to sync local directories or EC2 volumes with S3 buckets.

## Notifications
- **Google Cloud**: Uses Pub/Sub for object change notifications.
- **AWS Equivalent**: S3 Event Notifications can trigger AWS Lambda, SNS, or SQS when objects are created, deleted, or modified.

## Autoclass
- **Google Cloud**: Automatically manages storage class transitions.
- **AWS Equivalent**: S3 Intelligent-Tiering automatically moves data between access tiers based on usage patterns.

## Retention Lock
- **Google Cloud**: Object Retention Lock enforces retention policies for compliance.
- **AWS Equivalent**: S3 Object Lock provides WORM (Write Once Read Many) protection for regulatory compliance.

## Data Transfer Services
- **Google Cloud**:
    - `Transfer Appliance`: Physical device for large-scale data migration.
    - `Storage Transfer Service`: Online transfer from other clouds or HTTP sources.
    - `Offline Media Import`: Third-party service for physical media.
- **AWS Equivalent**:
    - `AWS Snowball/Snowmobile`: Physical devices for petabyte-scale transfers.
    - `AWS DataSync`: Online data transfer from on-prem or other clouds.
    - `AWS Transfer Family`: Supports SFTP, FTPS, and FTP for file transfers.

## Strong Consistency
- **Google Cloud**: Strong global consistency for reads, writes, and deletes.
- **AWS Equivalent**: Amazon S3 also provides strong read-after-write consistency for all objects.

## Cloud Storage Classes
Google Cloud offers four primary storage classes, each optimized for different access patterns and cost profiles:

| **Storage Class** | **Best For** | **Access Frequency** | **Min Storage Duration** | **Cost Characteristics** |
| :------ | :------ | :------ | :------ | :------ |
| **Standard** | Frequently accessed (hot) data	Daily or frequent | None | Highest cost, lowest latency |
| **Nearline** | Infrequently accessed data | ~Once/month | 30 days | Lower storage cost, higher access cost |
| **Coldline** | Rarely accessed data | ~Once/90 days | 90 days | Even lower storage cost, higher access cost |
| **Archive** | Archival data | <Once/year | 365 days | Lowest storage cost, highest access cost |

All classes offer:
- Unlimited storage
- No minimum object size
- Global availability
- High durability and low latency
- Uniform security and API experience
- Geo-redundancy in multi-region or dual-region setups

### Auto-Class
![gcp-storage-auto-class](/images/gcp/gcp-storage-auto-class.png)
- **What it does**: Automatically transitions objects between storage classes based on access patterns.
- Benefits:
    - Reduces storage costs by moving cold data to cheaper classes.
    - Moves hot data back to Standard for performance.
    - No manual intervention required.

### Security & Encryption
- **Server-side encryption**: Always enabled by default at no extra cost.
- **In-transit encryption**: Uses HTTPS/TLS for secure data transfer between client and Google Cloud.

## Data Transfer Options
| **Method** | **Description** | **Best For** |
| :------- | :------- | :----- |
| **Cloud Console (drag & drop)** | Manual upload via Chrome browser | Small files |
| **Cloud SDK (gsutil)** | CLI-based upload/download | Scripted or automated transfers |
| **Storage Transfer Service** | Scheduled batch transfers from other clouds, regions, or HTTPS endpoints | Large-scale online transfers |
| **Transfer Appliance** | Physical device shipped to Google for upload | Petabyte-scale offline transfers |

## Integration with Other Services
Cloud Storage integrates tightly with:
- **BigQuery**: Import/export tables
- **Cloud SQL**: Backup and restore
- **App Engine**: Store logs, static files, and app assets
- **Compute Engine**: Store startup scripts, images, and app data
- **Firestore**: Together with Cloud Storage, they form a common pattern in app development:
    - Store **metadata** (e.g., file name, owner, timestamp, access permissions) in **Firestore**
    - Store the **actual file** in **Cloud Storage**


# Filestore
Filestore is a fully managed Network Attached Storage (NAS) service on Google Cloud. It supports NFSv3 and is designed for applications that need a shared file system with predictable performance and scalability. Key features include:
- **NFSv3 compatibility**: Works with any NFSv3-compatible client.
- **Performance tuning**: Independently scale performance and capacity.
- **Use cases**: Enterprise app migration, media rendering, EDA, data analytics, genome sequencing, and web hosting.
- **No client-side software**: Native integration with Compute Engine and GKE.
- **Scale-out performance**: Supports hundreds of TBs and file locking.

## Key Differences: Filestore vs FSx vs EFS
| **Feature** | **Google Filestore** | **Amazon EFS** |
| :----- | :------ | :-------|
| **Protocol** | NFSv3 | NFSv4 |
| **Performance Tiers**	| Basic, High Scale	| Standard, One Zone |
| **Scalability** | Manual scaling | Elastic |
| **Backup** | Manual or via GCP tools | AWS Backup |
| **Use Cases** | HPC, media, analytics | Web apps, CMS, containers |
| **Compliance** | HIPAA, ISO, etc. | HIPAA, PCI-DSS |

# CloudSQL
Cloud SQL is a fully managed relational database service that supports:
- MySQL
- PostgreSQL
- SQL Server

It’s designed to offload operational tasks like:
- Patching and updates
- Backups and restores
- Replication setup
- High availability configuration

> **AWS Equivalent**: Amazon RDS (Relational Database Service)

## Key Features
| **Feature** | Description |
| :-------- | :-------- |
| **Fully Managed** | No need to install or maintain database software. |
| **Scalability** | Up to 128 vCPUs, 864 GB RAM, and 64 TB storage. |
| **Replication** | Supports automatic replication from Cloud SQL or external MySQL sources. |
| **Backups** | Managed backups included (7 backups covered in cost). |
| **Security** | Data encrypted at rest and in transit; includes network firewalls. |
| **Connectivity** | Accessible from App Engine, Compute Engine, and external apps using standard drivers. |

## Integration with Other Services
- App Engine: Use standard connectors like Connector/J (Java) or MySQLdb (Python).
- Compute Engine: Can be co-located in the same zone for low-latency access.
- External Tools: Compatible with SQL Workbench, Toad, and other SQL clients.

## Use Cases
- Traditional web and enterprise applications
- CMS platforms (e.g., WordPress, Drupal)
- Internal business apps
- Apps requiring strong consistency and relational structure

![gcp-cloud-sql-connection](/images/gcp/gcp-cloud-sql-connection.png)

# Cloud Spanner
**Cloud Spanner** is Google Cloud’s **fully managed, horizontally scalable, strongly consistent relational database**. It combines the benefits of traditional relational databases (like SQL support and ACID transactions) with the scalability and availability of NoSQL systems.

## Key Features
| **Feature** | **Description** |
| :------ | :------- |
| **Relational + SQL** | Supports ANSI SQL with joins, indexes, and transactions. |
| **Horizontal Scalability** | Automatically shards data across nodes to handle massive workloads. |
| **Strong Global Consistency** | Uses Google’s TrueTime API to ensure consistency across regions. |
| **High Throughput** | Handles tens of thousands of reads/writes per second. |
| **Built-in High Availability** | Offers 99.999% SLA with multi-region configurations. |
| **Fully Managed** | No need to manage replication, sharding, or failover. |

## Use Cases
- Global financial systems
- Inventory and order management
- Gaming backends
- Telecom billing systems
- Any application requiring relational structure + global scale

## Comparison with Other Google Cloud Databases
| **Feature** | **Spanner** | **Cloud SQL** | **Firestore** |
| :------- | :-------- | :------- | :------- |
| **Type** | Relational | Relational | NoSQL (document) |
| **Scale** | Global, horizontal | Regional, vertical | Global, horizontal
| **Consistency** | Strong | Strong | Eventual (by default) |
| **Use Case** | High-scale OLTP | Traditional apps | Mobile/web apps |

> **AWS Equivalent**: Amazon Aurora (for SQL) + DynamoDB (for scale), but neither offers the same global consistency and horizontal scaling in one service.

![gcp-choose-cloud-spanner](/images/gcp/gcp-choose-cloud-spanner.png)

# AlloyDB for PostgreSQL
AlloyDB is a fully managed, PostgreSQL-compatible database service designed for high-performance transactional and analytical workloads. It combines a custom Google-built database engine with a multi-node cloud-native architecture.
## Key Capabilities
- **Performance**: Up to 4x faster for transactional workloads and 100x faster for analytical queries than standard PostgreSQL.
- **Machine Learning Integration**: Built-in integration with Vertex AI for invoking ML models directly from SQL.
- **Automation**: Handles backups, replication, patching, and capacity management.
- **Adaptive Intelligence**: Uses ML for vacuum tuning, memory/storage optimization, and data tiering.
- **High Availability**: 99.99% SLA with automatic failover and multi-zone replication.

> **AWS Equivalent**: Amazon Aurora PostgreSQL

# Firestore
**Firestore** is a fully managed, serverless, NoSQL document database designed for mobile, web, and server applications. It offers real-time synchronization, offline support, and horizontal scalability.

> **AWS Equivalent**: Amazon DynamoDB (with some overlap with AWS AppSync for real-time sync)

## Data Model
- **Documents**: Store data as key-value pairs (e.g., firstname: "Aashish")
- **Collections**: Group of documents (e.g., users, orders)
- **Subcollections**: Nested collections within documents
- **Supports**: Complex nested objects and hierarchical data structures

## Querying
- Supports **NoSQL-style queries**:
    - Filter by fields
    - Combine filters and sorting
    - Chained conditions
- **Indexed by default**: Query performance scales with result size, not dataset size

## Real-Time Sync & Offline Support
- **Real-time updates**: Automatically syncs data across devices
- **Offline mode**: Apps can read/write/query data offline; syncs when reconnected
- **Caching**: Frequently accessed data is cached locally for performance

## Consistency & Transactions
- **Strong consistency**: Across all reads and writes
- **Multi-document transactions**: Atomic operations across multiple documents
- **Batch writes**: Group multiple operations into a single request

## Scalability & Availability
- **Horizontally scalable**: Handles millions of concurrent users
- **Multi-region replication**: Built-in high availability and durability
- **Serverless**: No infrastructure to manage

## Use Cases
| **Use Case** | **Why Firestore?** |
| :------- | :-------- |
| Mobile apps | Real-time sync, offline support |
| Web apps | Fast queries, flexible schema |
| IoT | Scalable, event-driven architecture |
| Chat/messaging | Real-time updates, low latency |
| Metadata store | Flexible document model |

![gcp-choose-firestore](/images/gcp/gcp-choose-firestore.png)

# Bigtable
**Cloud Bigtable** is Google Cloud’s **NoSQL wide-column database** designed for **massive scale, low latency**, and **high throughput**. It powers many of Google’s own services like Search, Maps, Gmail, and Analytics.

## Key Characteristics
| **Feature** | **Description** |
| :------- | :------- |
| **NoSQL** | Schema-less, wide-column format (similar to HBase) |
| **Massive Scale** | Handles petabytes of data and millions of operations per second |
| **Low Latency** | Consistent performance for real-time applications |
| **Horizontally Scalable** | Add nodes to increase throughput and storage |
| **High Availability** | Replication and failover support across zones and regions |
| **Strong Integration** | Works with Dataflow, Dataproc, Apache HBase API, and more |

## When to Use Bigtable
![gcp-choose-bigtable](/images/gcp/gcp-choose-bigtable.png)

Choose Bigtable if:
- You’re working with **>1 TB** of structured or semi-structured data
- Your data is **fast-changing** or **time-series** in nature
- You don’t need full relational features (e.g., joins, foreign keys)
- You need **real-time analytics**, **IoT ingestion**, or **ML pipelines**
- You’re running **batch or streaming** data processing

## Integration & Access
- **APIs & Clients**:
    - HBase client (Java)
    - HBase REST server
    - Google Cloud client libraries
- **Streaming**:
    - Dataflow (Apache Beam)
    - Spark Streaming
    - Apache Storm
- **Batch**:
    - Hadoop MapReduce
    - Apache Spark
    - Dataflow batch pipelines

## Common Use Cases
| **Use Case** | **Why Bigtable?** |
| :------ | :------- |
| IoT telemetry | High write throughput, time-series optimized |
| Financial tick data | Low latency, high volume |
| Real-time analytics | Fast reads/writes, scalable |
| ML feature stores | Structured, fast-access data |
| Personalization engines | High-speed lookups at scale |

# Comparing storage options
![storage-option-compare](/images/gcp/storage-option-compare.png)

# Google Memorystore
![gcp-memorystore](/images/gcp/gcp-memorystore.png)
