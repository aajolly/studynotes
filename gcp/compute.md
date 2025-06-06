# Compute Engine
**Compute Engine** is Google Cloud’s Infrastructure-as-a-Service (IaaS) offering. It allows you to create and run virtual machines (VMs) on Google’s infrastructure with high performance, scalability, and flexibility.

## Key Features
| **Feature** | **Description** |
| :------- | :------ |
| Fully Configurable VMs | Choose CPU, memory, storage, and OS (Linux, Windows, or custom images). |
| Deployment Options | Create VMs via Google Cloud Console, CLI (gcloud), or Compute Engine API. |
| Custom Machine Types | Define exact vCPU and memory requirements to optimize cost and performance. |
| Cloud Marketplace | Launch pre-configured solutions from Google and third-party vendors. |
| Billing | Per-second billing with a 1-minute minimum. | 

## Pricing Models
| **Model** | **Description** |
| :----- | :------ |
| Sustained Use Discounts | Automatically applied when VMs run >25% of the month. |
| Committed Use Discounts | Up to 57% off for 1- or 3-year commitments on vCPU and memory. |
| Preemptible VMs | Short-lived, cost-effective VMs (max 24 hours). |
| Spot VMs | Similar to Preemptible but with no max runtime and more features. |

![sustained-use-discounts](/images/gcp/sustained-use-discount.png)

## Storage & Performance
- High throughput between VMs and persistent disks is default—no special configuration or machine type required.
- Storage options are flexible and decoupled from compute.

## Cloud Marketplace
- Offers ready-to-deploy solutions (e.g., LAMP stack, WordPress).
- Some are free (usage-based billing only), others include licensing fees.
- Pricing estimates are shown before deployment.

## Machine Types in Compute Engine
You can configure VMs using:
- Predefined machine types: Standard, High-CPU, High-Memory, and more.
- Custom machine types: Specify exact vCPU and memory to optimize cost and performance.

## VM Access
| **OS** | **Access Method** | **Notes** |
| :---- | :------ | :------- |
| **Linux** | SSH via Cloud Console or gcloud | Creator has root access; can grant SSH to others |
| **Windows** | RDP using generated username/password | Credentials managed via Cloud Console |
> Firewall Rules: Required for SSH (TCP 22) and RDP (TCP 3389); preconfigured in the default network.

## VM Lifecycle States
![gcp-compute-lifecycle](/images/gcp/gcp-compute-lifecycle.png)

| **State** | **Description** |
| :------ | :------- |
| **Provisioning** | Resources (CPU, RAM, disk) are being allocated |
| **Staging** | IPs assigned, system image booted |
| **Running** | VM is active, supports live migration, snapshots, metadata changes |
| **Stopping/Terminated** | VM is shutting down or stopped; can be restarted or deleted |
| **Suspending/Suspended** | VM state saved to disk; can be resumed |
| **Repairing** | VM is temporarily unavailable due to internal error or host maintenance |
> **Reset**: Like pressing a reset button—clears memory but keeps VM running.
> **Shutdown Time**: ~90 seconds (30 seconds for preemptible VMs before forced off).

## Availability Policies
| **Policy** | **Default** | **Description** |
| :------ | :------ | :------ |
| **On Host Maintenance** | Live migrate | Can be changed to terminate |
| **Automatic Restart** | Enabled | VM restarts after crash or maintenance |
> Configurable during or after VM creation.

## OS Patch Management
- **Premium OS Images**: Include patch management and licensing costs.
- **Patch Management Tools**:
    - **Patch Compliance Reporting**: Shows patch status and recommendations.
    - **Patch Deployment**: Automates patching across VM fleets.
- **Patch Management Features**:
    - **Patch Approvals**: Select which patches to apply.
    - **Flexible Scheduling**: One-time or recurring jobs.
    - **Advanced Configs**: Pre/post patch scripts.
    - **Centralized Management**: Manage all patch jobs from one place.

## Billing Considerations
- **Stopped VMs**: No charge for CPU/RAM, but disks and static IPs still incur charges.
- **Terminated VMs**: Allow changes like machine type, but not the base image.
![gcp-terminated-vm-actions](/images/gcp/gcp-compute-actions.png)

## Compute Options
![gcp-compute-machine-type](/images/gcp/compute-machine-type.png)
![compute-machine-families](/images/gcp/compute-machine-families.png)
![compute-general-purpose-family](/images/gcp/general-purpose-machine-family.png)
![compute-optimized-machine-family](/images/gcp/compute-optimized-machine-family.png)
![memory-optimized-machine-family](/images/gcp/memory-optimized-machine-family.png)
![acclerator-optimized-machine-family](/images/gcp/accelerator-optimized-machine-family.png)
![custom-machine-type](/images/gcp/custom-machine-type.png)

## Special Compute Configurations
### Sole-Tenant Nodes
- Dedicated physical servers for your project
- Use cases:
    - Compliance (e.g., PCI, HIPAA)
    - Licensing (BYOL)
    - Workload isolation
- Benefits:
    - Physical separation from other tenants
    - Support for custom machine types
    - In-place restart to optimize core usage

### Shielded VMs
- Enhanced security against rootkits and boot-level malware
- Features:
    - Secure Boot
    - vTPM (virtual Trusted Platform Module)
    - Integrity Monitoring
- Use case: When you need verifiable integrity of your VM’s boot process
> Part of Google’s Shielded Cloud Initiative

### Confidential VMs
- Encrypts data in use (not just at rest or in transit)
- Powered by: AMD EPYC “Rome” processors with SEV (Secure Encrypted Virtualization)
- Use case:
    - Protecting sensitive data during processing
    - Secure collaboration without code changes
- Benefits:
    - Inline memory encryption
    - No performance compromise
    - Google has **no access** to encryption keys

## Images
### VM Boot Disk Images
- **Contents**: Boot loader, OS, file system, pre-configured software, and customizations.
- **Types**: Public or custom; Linux and Windows options available.
- **Premium Images**:
    - Charged per second after 1-minute minimum.
    - SQL Server images: charged per minute after 10-minute minimum.
    - Prices vary by machine type but are globally consistent.

### Custom Images
- Created by pre-installing authorized software.
- Can be imported from on-premises, workstations, or other cloud providers (no cost).
- Shareable across projects.

### Machine Images
- Store full VM configuration, metadata, permissions, and disk data.
- Ideal for:
    - VM creation
    - Backup and recovery
    - Instance cloning and replication

## Disk Options
![boot-disk](/images/gcp/boot-disk.png)
![persistent-disk](/images/gcp/persistent-disk.png)
![local-ssd](/images/gcp/local-ssd.png)
![ram-risk](/images/gcp/ram-disk.png)
![summary](/images/gcp/summary-disk-options.png)

## Common Compute Engine Actions
### Metadata Server
- Stores VM instance metadata.
- Useful with startup and shutdown scripts.
- Allows programmatic access to instance-specific data (e.g., external IP) without extra authorization.
- Promotes reusable and less brittle scripts.

### Moving VM Instances
- Reasons: geographic optimization or zone deprecation.
- Steps:
    - Shut down the VM.
    - Move it to the new zone or region.
    - Restart the VM.
    - Update any references to the original VM.
- Use a machine image to recreate the VM in the new location.

### Snapshots
- Use Cases:
    - Backup critical data.
    - Migrate data between zones.
    - Transfer data to different disk types (e.g., HDD → SSD).
- Only available for persistent disks (not local SSDs).
- Incremental, compressed, and cost-effective.
- Can be scheduled for regular backups.
- Snapshots can be restored to new disks for zone migration.

### Resizing Persistent Disks
- Increases storage and improves I/O performance.
- Can be done while the disk is attached and the VM is running.
- Disks can only be expanded, not reduced in size.

## Autoscaling
- **What it does**: Automatically adjusts the number of VM instances in a managed instance group based on load metrics like CPU utilization or custom metrics.
- **Why it matters**: Helps maintain performance while optimizing cost.

## Scaling Up vs. Scaling Out
- Scaling out (adding more VMs) is the common starting point.
- Scaling up (larger VMs) is used for workloads like:
    - In-memory databases
    - CPU-intensive analytics

## Quotas and Limits
- The maximum number of CPUs per VM depends on:
    - The machine family (e.g., N2, E2, C2)
    - Your project’s quota, which is zone-specific
- You can request quota increases via the Google Cloud Console.
> For current specs: https://cloud.google.com/compute/docs/machine-types

### Managed Instance Group (MIG)
A Managed Instance Group is a collection of identical VM instances managed as a single entity using an instance template.

#### Key Features of MIGs
| **Feature** | **Description** |
| :-------- | :--------- |
| Rolling Updates | Easily update all instances by applying a new instance template. |
| Autoscaling | Automatically scales the number of instances based on load. |
| Load Balancing | Integrates with load balancers to distribute traffic across instances. |
| Self-Healing | Automatically recreates failed or deleted instances using the same name and template. |
| Health Checks | Identifies and replaces unhealthy instances to maintain optimal performance. |
| Zonal vs Regional | Regional MIGs are preferred for high availability as they span multiple zones. |

#### MIG Creation Workflow
1. Create an Instance Template – Defines the configuration for VMs.
2. Create a MIG – Specify number of instances and use the template.
3. Configure Group Settings:
    - Stateless (e.g., web frontends) or Stateful (e.g., databases).
    - Zonal or Regional deployment.
    - Port mappings (optional).
    - Autoscaling policies.
    - Health checks for traffic routing.

#### Autoscaling in Google Cloud MIGs
**Purpose**: Automatically adjusts the number of VM instances in a MIG based on demand to optimize performance and cost.

**Key Features**:
- Policy-based scaling:
    - CPU utilization (e.g., target 75%)
    - Load balancing capacity
    - Cloud Monitoring metrics
    - Queue-based workloads (e.g., Pub/Sub)
    - Scheduled scaling (start time, duration, recurrence)
- Monitoring:
    - View metrics like CPU, disk, and network usage per instance or group.
    - Integrates with Cloud Monitoring for alerts and visualization.
> **Example**: If two instances are at 100% and 85% CPU, and the target is 75%, the autoscaler adds another instance to balance the load.

#### Health Checks in MIGs
**Purpose**: Ensure only healthy instances receive traffic and enable autohealing.

**Configuration Parameters**:
- Protocol & Port: e.g., HTTP on port 80
- Check Interval: How often to check (e.g., every 5s)
- Timeout: How long to wait for a response (e.g., 5s)
- Healthy Threshold: Number of successful checks to mark healthy (e.g., 2)
- Unhealthy Threshold: Number of failed checks to mark unhealthy (e.g., 2)
> **Autohealing**: Unhealthy instances are automatically recreated using the same template.

#### Stateful IP Configuration
**Purpose**: Preserve internal/external IPs during updates, autohealing, or recreation.

**Use Cases**:
- Applications requiring static IPs
- IP-dependent configurations
- Migration scenarios without changing network settings
**Options**:
- Automatically promote ephemeral IPs to static
- Assign specific IPs to instances
- Apply stateful policy to existing and future instances

# Containers
**Containers** are a lightweight, portable way to package and run applications and their dependencies in isolated environments. They provide the flexibility of Infrastructure as a Service (IaaS) with the scalability of Platform as a Service (PaaS).

## Containers vs. Virtual Machines (VMs)
| **Feature** | **Virtual Machines** | **Containers** |
| :------ | :------- | :-------- |
| **Isolation** | Full OS per app | Shared OS kernel |
| **Startup Time** | Minutes | Seconds |
| **Size** | GBs (includes OS) | MBs (app + dependencies) |
| **Scalability** | Slower, heavier | Fast, lightweight |
| **Portability** | Limited | High (runs anywhere with container runtime) |

## How Containers Work
- A container wraps your code + dependencies into a single unit.
- It runs on a shared OS kernel using a container runtime (e.g., Docker).
- Containers are process-level isolated, not full OS virtualized.
- They can be started, stopped, and scaled very quickly.

## Benefits of Containers
- **Portability**: Move from dev → staging → production without changes.
- **Efficiency**: Use fewer resources than VMs.
- **Speed**: Start in seconds, ideal for scaling.
- **Modularity**: Break apps into microservices, each in its own container.
- **Scalability**: Easily scale individual services across hosts.

## Use Case Example
- You want to scale a web server:
    - With VMs: You clone and boot entire OS instances.
    - With containers: You spin up dozens of lightweight containers in seconds.

## Microservices Architecture
- Build apps as collections of containers, each handling a specific function.
- Connect them via networking.
- Scale and deploy independently.
- Run across multiple hosts with orchestration (e.g., Kubernetes).

## Kubernetes
**Kubernetes** is an open-source platform for **orchestrating containerized applications**. It automates deployment, scaling, and management of containers across clusters of machines.

### Key Concepts in Kubernetes
| **Concept** | **Description** |
| :------- | :-------- |
| **Cluster** | A set of nodes (VMs) managed by a control plane. |
| **Node** | A compute instance (e.g., VM) that runs containerized workloads. |
| **Pod** | The smallest deployable unit in Kubernetes. Typically runs one container, but can run multiple tightly coupled containers. |
| **Deployment** | A controller that manages a group of pods, ensuring the desired number of replicas are running. |
| **Service** | An abstraction that defines a logical set of pods and a policy to access them. Provides a stable IP address and DNS name. |
| **Load Balancer** | Exposes services to external traffic. In GKE, this is automatically provisioned. |
| **kubectl** | The command-line tool to interact with Kubernetes clusters. |

### How GKE Simplifies Kubernetes
**Google Kubernetes Engine (GKE)** is a managed Kubernetes service that:
- Bootstraps and manages the Kubernetes control plane.
- Handles upgrades, patching, and scaling.
- Integrates with Google Cloud services (e.g., IAM, Cloud Monitoring, Cloud Load Balancing).

### Deployment Lifecycle
1. Create a Pod: `kubectl run`
2. Create a Deployment: Manages pod replicas and updates.
3. Expose a Service: `kubectl` expose to create a stable endpoint.
4. Scale: `kubectl scale` or use autoscaling based on CPU/memory.
5. Update: Use `kubectl rollout` or update the deployment config and apply with `kubectl apply`.

### Imperative vs Declarative
| **Mode** | **Description** |
| :------- | :--------- |
| **Imperative** | Direct commands (e.g., kubectl run, kubectl scale) |
| **Declarative** | Define desired state in YAML/JSON config files and apply with kubectl apply |

### Rolling Updates
- Kubernetes supports rolling updates to minimize downtime.
- New pods are created before old ones are terminated.
- Controlled via deployment strategies in the config file.

### Use Cases
- Microservices architecture
- CI/CD pipelines
- Scalable web applications
- Real-time data processing

### GKE Architecture Overview
- **Cluster**: A group of Compute Engine VMs (nodes) managed by Kubernetes.
- **Control Plane**: Managed by Google in GKE—handles scheduling, scaling, and health monitoring.
- **Node Pools**: Subsets of nodes within a cluster, useful for workload separation and flexibility.

### GKE Modes
| **Mode** | **Description** | Who Manages What |
| **Autopilot** | Fully managed, production-optimized mode | Google manages infrastructure, scaling, upgrades, security |
| **Standard** | More control and customization | You manage node configuration, scaling, and maintenance |
> **Recommendation**: Use Autopilot unless you need fine-grained control over infrastructure.

### Key Features of GKE
- **Managed Control Plane**: No need to provision or manage Kubernetes masters.
- **Autoscaling**: Automatically adjusts the number of nodes or pods based on demand.
- **Auto-upgrades & Auto-repair**: Keeps your cluster secure and healthy.
- **Load Balancing**: Integrated with Google Cloud Load Balancer for external access.
- **Logging & Monitoring**: Built-in integration with Google Cloud Observability.
- **Declarative Management**: Use YAML config files and kubectl apply to define desired state.

### Common Commands
| **Task** | **Command** |
| :------- | :-------- |
| Create a cluster | `gcloud container clusters create k1` |
| View pods | `kubectl get pods` |
| Scale deployment| `kubectl scale deployment <name> --replicas=3` |
| Update app | `kubectl rollout restart deployment <name>` |
| Apply config | `kubectl apply -f deployment.yaml` |
| Get service IP | `kubectl get services` |

## Cloud Run
**Cloud Run** is a fully managed, serverless platform that runs **stateless containers** in response to **HTTP requests** or **Pub/Sub events**. It abstracts away infrastructure management so developers can focus on writing code.

> **Built on**: https://knative.dev/, an open-source Kubernetes-based platform for serverless workloads.

### Key Features
| **Feature** | **Description** |
| :--------- | :------- |
| **Serverless** | No need to manage servers, clusters, or scaling. |
| **Fast Scaling** | Scales from zero to thousands of containers instantly. |
| **Pay-per-use** | Billed per 100ms of container execution time. |
| **HTTPS by Default** | Each service gets a secure, unique URL. |
| **Flexible Deployment** | Deploy from container image or source code (via Buildpacks). |
| **Language Agnostic** | Supports any language or binary compiled for Linux 64-bit. |

### Developer Workflow
1. **Write** your app in any language (e.g., Python, Go, Java, Node.js).
2. **Package** it into a container image.
3. **Deploy** it to Cloud Run via:
    - Container image (pushed to Artifact Registry)
    - Source code (Cloud Run builds it using Buildpacks)

Once deployed, Cloud Run:
- Assigns a **unique HTTPS endpoint**
- **Auto-scales** based on incoming traffic
- **Shuts down** when idle (no cost during idle time)

### Deployment Options
| **Option** | **Description** |
| :------ | :------- |
| **Fully Managed** | Google handles all infrastructure. |
| **GKE-based** | Run Cloud Run on GKE for more control. |
| **Anywhere Knative Runs** | Deploy to your own Kubernetes cluster using Knative. |

### Pricing Model
- **Granular billing**: Charged per 100ms of CPU, memory, and request count.
- **No charge** when containers are idle.
- **Request-based fee**: Small fee per 1 million requests.
- **Resource-based fee**: Higher CPU/memory = higher cost.

### Use Cases
- REST APIs and microservices
- Event-driven processing (e.g., Pub/Sub)
- Webhooks and backend services
- Lightweight data processing tasks
- Running legacy binaries in a modern environment