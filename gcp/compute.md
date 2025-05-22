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