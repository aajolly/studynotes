# BigQuery
- BigQuery is Google Cloud’s serverless, highly scalable, and cost-effective cloud data warehouse.
- Designed for petabyte-scale analytics with super-fast SQL queries.
- No infrastructure management required—ideal for analysts and engineers.

## Key Features
| **Feature** | **Description** |
| :------ | :------ |
| Serverless | No need to manage clusters or VMs. |
| SQL Interface | Use standard SQL for querying data. |
| Massive Scalability | Handles petabytes of data with ease. |
| Multiple Access Methods | Access via Cloud Console, CLI, REST API, or client libraries (Java, .NET, Python). |
| Third-party Integration | Compatible with BI tools for visualization and ETL. |

> AWS Equivalent: Amazon Redshift

# Cloud Dataflow
Cloud Dataflow is a fully managed service for executing a wide variety of data processing patterns. It supports both stream and batch processing with equal reliability and flexibility.

## Key Features
| **Feature** | **Description** |
| :------ | :------ |
| Managed Infrastructure | No need to manage servers or clusters. |
| Auto-scaling | Dynamically scales to handle millions of events per second. |
| Unified Programming Model | Uses Apache Beam SDK (Java, Python, SQL) for pipeline development. |
| Windowing & Sessions | Built-in support for complex event time processing. | 
| Monitoring | Integrated with Cloud Monitoring (formerly Stackdriver) for alerts and logs. |

## Data Sources & Sinks
- Sources: Cloud Pub/Sub, Cloud Datastore, Apache Kafka, Apache Avro, etc.
- Sinks: BigQuery, Cloud Bigtable, AI Platform, Data Studio (for real-time dashboards).

## Common Use Cases
- Real-time analytics (e.g., IoT telemetry).
- ETL pipelines (Extract, Transform, Load).
- Sessionization and windowed aggregations.
- Data enrichment and cleansing.

> AWS Equivalent: AWS Glue (for batch ETL), Amazon Kinesis Data Analytics (for stream processing), AWS Step Functions (for orchestration)
![gcp-dataflow](/images/gcp/gcp-dataflow.png)

# Cloud Dataprep
Cloud Dataprep is a serverless, intelligent data preparation service for visually exploring, cleaning, and transforming structured and unstructured data. It’s designed for analysts and data engineers who want to prepare data without writing code.

## Key Features
| **Feature** | **Description** |
| :------ | :------ |
| Visual Interface | Drag-and-drop UI with predictive transformation suggestions. |
| No Code Required | Automatically suggests transformations based on user input. |
| Schema & Anomaly Detection | Auto-detects data types, joins, and anomalies. |
| Serverless & Scalable | No infrastructure to manage; scales on demand. |
| Integrated with GCP | Works with BigQuery, Cloud Storage, and Cloud Dataflow. |
| Powered by Trifacta | Based on Trifacta Wrangler, integrated directly into GCP. |

![gcp-dataprep](/images/gcp/gcp-dataprep.png)

## Common Use Cases
- Data cleansing and enrichment before analytics.
- Preprocessing for machine learning pipelines.
- Exploratory data analysis and profiling.

> AWS Equivalent: AWS Glue DataBrew

# Cloud Dataproc
Cloud Dataproc is a fully managed, fast, and cost-effective service for running Apache Spark, Hadoop, Hive, and Pig clusters on Google Cloud. It simplifies big data processing by automating cluster management and scaling.

## Key Features
| **Feature** | **Description** |
| :------ | :------ |
| Fast Cluster Lifecycle | Clusters start, scale, and shut down in ~90 seconds. |
| Per-Second Billing | Pay only for what you use. |
| Preemptible VM Support | Reduce costs by using short-lived, low-cost VMs. |
| Built-in GCP Integration | Works seamlessly with BigQuery, Cloud Storage, Cloud Bigtable, and Cloud Monitoring. |
| Familiar Tools | Use existing Spark, Hadoop, Hive, or Pig jobs without rewriting. |

![gcp-dataflow-vs-dataproc](/images/gcp/dataflow-vs-dataproc.png)

> AWS Equivalent: Amazon EMR