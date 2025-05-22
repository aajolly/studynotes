# Cloud Load Balancing?
Cloud Load Balancing is a fully managed, software-defined service that distributes user traffic across multiple backend instances to ensure high availability, scalability, and performance.
- No pre-warming required: It automatically scales to handle sudden spikes in traffic.
- Fully distributed: Not tied to specific VMs—no need to manage or scale the load balancers themselves.
- Supports multiple protocols: HTTP(S), TCP, SSL, and UDP.
- Cross-region failover: Automatically shifts traffic if a backend becomes unhealthy.

## Why Use Load Balancing?
- Ensures reliable access to applications even as the number of VMs changes (e.g., from 4 to 40).
- Helps maintain performance and uptime by spreading traffic evenly.
- Reacts quickly to changes in:
    - User demand
    - Network conditions
    - Backend health

## Types of Load Balancers in Google Cloud
| **Type** | **OSI Layer** | **Description** |
| :--- | :------- | :------- |
| Application Load Balancer | Layer 7 | Handles HTTP/HTTPS traffic. Supports content-based routing, SSL termination, and reverse proxying. Can be external or internal. |
| Proxy Network Load Balancer | Layer 4 | Terminates client connections and opens new ones to backends. Supports hybrid and multi-cloud backends. |
| Passthrough Network Load Balancer | Layer 4 | Forwards traffic directly to backend without modifying it. Preserves source IP. Ideal for direct server return.|
![gcp-lb-l7](/images/gcp/gcp-lb-l7.png)
![gcp-lb-l4-proxy](/images/gcp/gcp-lb-l4-proxy.png)
![gcp-lb-l4-passthrough](/images/gcp/gcp-lb-l4-passthrough.png)

## Key Features
- **Autoscaling integration**: Works seamlessly with managed instance groups.
- **Health checks**: Automatically routes traffic away from unhealthy instances.
- **Global and regional options**: Choose based on latency, redundancy, and compliance needs.
- **Traffic splitting**: Useful for gradual rollouts and A/B testing.

# Google Cloud’s network connectivity options
## Cloud VPN (Over the Internet)
- **What it is**: Creates a secure IPsec VPN tunnel between your Google VPC and another network (e.g., on-premises).
- **With Cloud Router**: Enables dynamic route exchange using BGP (Border Gateway Protocol).
- **Use Case**: Quick, cost-effective setup for secure connectivity.
> **AWS Equivalent**: AWS Site-to-Site VPN + AWS Transit Gateway

## Direct Peering
- **What it is**: Connects your on-premises router directly to Google at a Google Point of Presence (PoP).
- **Use Case**: Low-latency access to Google services without traversing the public internet.
- **Limitation**: Not covered by a Google SLA.
> **AWS Equivalent**: AWS Direct Connect Public VIF

## Carrier Peering
- **What it is**: Connects your on-premises network to Google via a partner service provider.
- **Use Case**: When you're not in a Google PoP but want direct access to Google services.
- **Limitation**: No SLA from Google.
> **AWS Equivalent**: AWS Direct Connect via Partner

## Dedicated Interconnect
- **What it is**: Provides private, high-throughput, SLA-backed connectivity between your network and Google.
- **Speeds**: 10 Gbps or 100 Gbps.
- **SLA**: Up to 99.99% if topology meets Google’s requirements.
- **Backup**: Can be paired with Cloud VPN for redundancy.
> **AWS Equivalent**: AWS Direct Connect Dedicated

## Partner Interconnect
- **What it is**: Similar to Dedicated Interconnect but delivered via a Google-approved service provider.
- **Use Case**: When your data center can’t reach a Dedicated Interconnect location or doesn’t need full 10 Gbps.
- **SLA**: Up to 99.99%, but Google is not responsible for the partner’s portion.
> **AWS Equivalent**: AWS Direct Connect Hosted

## Cross-Cloud Interconnect
- **What it is**: Provides dedicated, high-bandwidth connectivity between Google Cloud and another cloud provider.
- **Speeds**: 10 Gbps or 100 Gbps.
- **Use Case**: Multicloud strategies requiring low-latency, secure, and high-throughput inter-cloud communication.
- Benefits:
    - Simplified setup
    - Site-to-site encryption
    - Reduced complexity
> **AWS Equivalent**: AWS Cloud WAN (partial), or third-party multicloud interconnect solutions