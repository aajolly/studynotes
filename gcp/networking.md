# Projects
- Projects are the top-level containers for all Google Cloud resources.
- They:
    - Associate resources with billing
    - Contain networks, VMs, storage, and more
    - Have a default quota of 15 VPC networks, which can be increased

# VPC Networks
- Global in scope: span all Google Cloud regions
- Types of VPC networks:
| **Type** | **Description** |
| :----- | :------- |
| **Default** | Auto mode network with pre-created subnets and firewall rules |
| **Auto Mode** | Automatically creates one subnet per region with predefined IP ranges |
| **Custom Mode** | You define subnets, IP ranges, and regions manually (more control) |

## Key Characteristics:
- No IP range of their own—composed of subnet IPs
- Can be shared across projects or peered with other VPCs
- Support IPv6 in custom mode (e.g., dual-stack VMs)

## Subnetworks (Subnets)
- **Regional** in scope: span all zones within a region
- **Customizable**:
    - You define **CIDR blocks** (non-overlapping)
    - Can **expand** IP ranges without downtime (prefix must be smaller)
- **Reserved IPs** in each subnet:
    - `.0` – Network address
    - `.1` – Subnet gateway
    - `Second-to-last` – Reserved
    - `Last` – Broadcast address

## VM Communication
- **Same network, different regions**: VMs can communicate via internal IPs
- **Different networks, same region**: VMs must use external IPs (still routed privately via Google’s edge routers)
- **Global private communication**: VMs in different regions can communicate securely via VPN over Google’s private network

## Subnet Expansion Rules
- Must not **overlap** with other subnets
- Must be a **valid CIDR block**
- Cannot span **multiple RFC ranges**
- **Auto mode** subnets start at /20, expandable to /16
- **Custom mode** allows more flexibility and larger ranges

## Best Practices
- Use **custom mode** for production environments
- Avoid overly large subnets to prevent **CIDR collisions**
- Use **shared VPCs** for centralized network management
- Use **firewall rules** at the network level for consistent policy enforcement

## IP Addresses
![gcp-ip-addresses](/images/gcp/gcp-ip-addresses.png)

### External vs Internal IP Addresses
- **Internal IP**: Assigned from a subnet’s CIDR range; visible to the VM OS (e.g., via ifconfig).
- **External IP**: Can be ephemeral or static, but is not visible to the VM OS. It’s mapped transparently by the VPC.
> The OS only sees the internal IP. The VPC handles NAT (Network Address Translation) for external access.

### DNS Resolution in Google Cloud
- Internal DNS
    - Zonal DNS (recommended): Higher reliability; isolates failures to individual zones.
    - Global DNS: Project-wide, but less fault-tolerant.

Each VM has:
- A hostname (same as instance name)
- A fully qualified domain name (FQDN):
    - Format: hostname.c.<project-id>.internal
- DNS names remain stable even if the internal IP changes.

#### Metadata Server as DNS Resolver
- Each VM uses the metadata server (169.254.169.254) for DNS resolution.
- It:
    - Resolves internal names
    - Forwards public DNS queries to Google Public DNS

#### External DNS
- External IPs can be used to connect to VMs from outside the project.
- Public DNS records are not created automatically.
    - Admins can publish them manually using Cloud DNS or other DNS providers.

#### Cloud DNS
- Managed, scalable, and authoritative DNS service.
- Built on Google’s global Anycast network.
- Offers:
    - Low-latency, high-availability DNS resolution
    - 100% uptime SLA
    - Support for millions of DNS records
    - Management via UI, CLI, or API
> **Use Case**: Host DNS zones for your domains and integrate with GCP services.

#### Alias IP Ranges
- Allow assigning multiple internal IPs to a single VM interface.
- Useful for:
    - Hosting multiple services or containers on one VM
    - Assigning separate IPs to each service
    - Alias IPs are drawn from the primary or secondary CIDR ranges of the subnet.

#### Subnet IP Addressing Notes
- Each subnet reserves 4 IPs:
    - `.0` – Network address
    - `.1` – Gateway
    - `Second-to-last` – Reserved
    - `Last` – Broadcast

#### IP Addresses for Default Domains
- Google publishes the complete list of IP ranges that it announces to the internet in goog.json.
Google also publishes a list of Google Cloud customer-usable global and regional external IP
addresses ranges in cloud.json.
- The following files replace the _spf.google.com TXT records previously recommended to use for
listing Google IP addresses.
    - https:/ /www.gstatic.com/ipranges/cloud.json provides a JSON representation of Cloud
IP addresses organized by region.
    - https:/ /www.gstatic.com/ipranges/cloud_geofeed is a standard geofeed formatted IP
geolocation file that we share with 3rd-party IP geo providers like Maxmind, Neustar, and
IP2Location.
    - https:/ /www.gstatic.com/ipranges/goog.json and https:/ /www.gstatic.com/ipranges/goog.txt are JSON and TXT formatted files respectively that include Google public prefixes in CIDR notation.

> For more information as well as an example of how to use this information, refer to https:/ /cloud.google.com/vpc/docs/configure-private-google-access#ip-addr-defaults

## Routing in Google Cloud VPC
- Default Routing Behavior
    - Every VPC network includes:
        - **Local subnet routes**: Allow communication between VMs in the same subnet.
        - **Network-wide routes**: Allow communication between VMs across subnets in the same VPC.
        - **Default internet route**: Sends traffic to destinations outside the VPC via the default internet gateway.
- Custom Routes
    - You can create **custom static routes** to override default behavior.
    - Routes are matched based on **destination IP address**.
    - A route applies to an instance if:
        - The route’s **network** matches the instance’s network.
        - The route’s **tags** (if any) match the instance’s tags.
- Routing Table
    - Each VM has a **read-only routing table** derived from the VPC’s route collection.
    - All traffic from a VM is first processed by a **virtual router** that determines the next hop.

## Firewall Rules in Google Cloud
- **Key Characteristics**
    - **Stateful**: Once a connection is allowed, return traffic is automatically allowed.
    - **Distributed**: Applied at the instance level, even though defined at the network level.
    - **Default Rules**:
        - Implied deny all ingress
        - Implied allow all egress

### Firewall Rule Components
| **Component** | **Description** |
| :--------- | :------- |
| **Direction** | Ingress (incoming) or Egress (outgoing) |
| **Source/Destination** | CIDR ranges for ingress/egress |
| **Protocol/Port** | TCP, UDP, ICMP, etc. |
| **Action**| Allow or Deny |
| **Priority** | Lower number = higher priority |
| **Target** | All instances or those with specific tags/service accounts |

### Use Cases
- **Egress** Rules
    - Control outbound traffic from VMs.
    - Example: Prevent a VM from accessing external IPs or internal services.
- **Ingress** Rules
    - Control inbound traffic to VMs.
    - Example: Allow SSH (TCP port 22) from a specific IP range.

## DNS and IP Addressing
- **Internal DNS**
    - Zonal (recommended) and Global
    - Hostnames resolve to internal IPs
- **External DNS**
    - Not automatically published
    - Can be managed via Cloud DNS
- **Alias IPs**
    - Assign multiple internal IPs to a single VM interface
    - Useful for containerized workloads

## Pricing
![gcp-network-pricing1](/images/gcp/gcp-network-pricing1.png)
![gcp-network-pricing2](/images/gcp/gcp-network-pricing2.png)

## Common Network Designs
### High Availability Design (Zonal Redundancy)
- **Goal**: Improve availability without added complexity.
- **How**:
    - Deploy VMs in **multiple zones** within the **same region** and **same subnet**.
    - Use a **regional managed instance group (MIG)** to distribute instances across zones.
    - Apply **firewall rules** at the subnet level (e.g., 10.2.0.0/16) to simplify security.
- **Benefit**: Increased availability with minimal configuration overhead.

### Globalization Design (Regional Redundancy)
- **Goal**: Maximize fault tolerance and performance for global users.
- **How**:
    - Deploy resources in multiple regions.
    - Use a global external Application Load Balancer to:
        - Route users to the nearest healthy region
        - Improve latency and resilience
    - Combine with regional MIGs for zone-level redundancy within each region.
- **Benefit**: Resilient, low-latency experience for global users.

### Secure Internet Access with Cloud NAT
- **Goal**: Allow private VMs to access the internet securely.
- **How**:
    - Assign only internal IPs to VMs.
    - Use Cloud NAT for outbound internet access (e.g., updates, patching).
    - No inbound access is allowed—Cloud NAT is outbound-only.
- **Benefit**: Secure internet access without exposing VMs to public IPs.

### Accessing Google APIs Privately with Private Google Access
- **Goal**: Allow internal-only VMs to access Google APIs (e.g., Cloud Storage).
- **How**:
    - Enable Private Google Access on the subnet.
    - VMs without external IPs can then reach Google APIs via internal routes.
- **Benefit**: Secure API access without public IPs.

> **Google Private Access** is conceptually similar to **AWS VPC Gateway Endpoints**, but there are some key differences in scope and implementation.

### Comparison: Google Private Access vs AWS Gateway Endpoints
| **Feature** | **Google Private Access** | **AWS VPC Gateway Endpoints** |
| :------ | :-------- | :------- | :------ |
| **Purpose** | Allow VMs with only internal IPs to access Google APIs and services | Allow EC2 instances to privately access AWS services (e.g., S3, DynamoDB) without using public IPs |
| **Applies To** | Google APIs and services (e.g., Cloud Storage, BigQuery)	| AWS services (S3, DynamoDB) |
| **Network Scope** | Enabled at the subnet level | Configured per VPC and route table |
| **Traffic Path** | Stays within Google’s internal network | Stays within AWS’s internal network |
| **Security Benefit** | No need for external IPs; reduces exposure | No need for NAT Gateway or Internet Gateway |
| **Billing Benefit** | Avoids egress charges for public internet | Avoids NAT Gateway data processing charges |

# Cloud Load Balancing
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
![gcp-lb-options](/images/gcp/gcp-lb-options.png)
| **Type** | **OSI Layer** | **Description** |
| :--- | :------- | :------- |
| Application Load Balancer | Layer 7 | Handles HTTP/HTTPS traffic. Supports content-based routing, SSL termination, and reverse proxying. Can be external or internal. |
| Proxy Network Load Balancer | Layer 4 | Terminates client connections and opens new ones to backends. Supports hybrid and multi-cloud backends. |
| Passthrough Network Load Balancer | Layer 4 | Forwards traffic directly to backend without modifying it. Preserves source IP. Ideal for direct server return.|
![http-lb-architecture](/images/gcp/https_lb_architecture.png)
![gcp-lb-l7](/images/gcp/gcp-lb-l7.png)
![gcp-lb-l4-proxy](/images/gcp/gcp-lb-l4-proxy.png)
![gcp-lb-l4-passthrough](/images/gcp/gcp-lb-l4-passthrough.png)

## Key Features
- **Autoscaling integration**: Works seamlessly with managed instance groups.
- **Health checks**: Automatically routes traffic away from unhealthy instances.
- **Global and regional options**: Choose based on latency, redundancy, and compliance needs.
- **Traffic splitting**: Useful for gradual rollouts and A/B testing.

# Cloud CDN
Cloud CDN (Content Delivery Network) uses Google’s globally distributed edge points of presence (PoPs) to cache HTTP(S) load-balanced content closer to users. There are over 90 cache sites across Asia Pacific, Americas, and EMEA.

## Why Use Cloud CDN?
- Faster content delivery by caching at the edge.
- Reduced serving costs by offloading traffic from origin servers.
- Easy to enable via a checkbox in the backend service of an HTTP(S) load balancer.

## How Cloud CDN Works (Response Flow)
1. **User Request**: A user in San Francisco requests content.
2. **Cache Miss**: If the local cache doesn’t have it, it checks nearby caches (e.g., Los Angeles).
3. **Backend Fetch**: If still unavailable, the request is forwarded to the HTTP(S) load balancer, which routes it to:
    - A Cloud Storage bucket (e.g., for static content).
    - A Managed instance group (e.g., for dynamic PHP content).
4. **Cache Store**: If the content is cacheable, it’s stored at the San Francisco cache site.
5. **Cache Hit**: Future requests for the same content are served directly from the cache.

## Logging and Monitoring
- Each request is logged with a Cache Hit or Cache Miss status.
- Logs help analyze cache effectiveness and troubleshoot issues.

## Cache Modes
Cloud CDN offers three cache modes to control caching behavior:
| **Mode** | **Behavior** |
| :------ | :-------- |
| USE_ORIGIN_HEADERS | Respects cache headers from the origin. |
| CACHE_ALL_STATIC | Automatically caches static content unless marked no-store, private, etc. |
| FORCE_CACHE_ALL | Caches all responses, ignoring origin cache headers. Use with caution. |
> Avoid caching private or per-user content with FORCE_CACHE_ALL.

# Google Cloud’s network connectivity options
## Cloud VPN (Over the Internet)
- **What it is**: Creates a secure IPsec VPN tunnel between your Google VPC and another network (e.g., on-premises).
- **With Cloud Router**: Enables dynamic route exchange using BGP (Border Gateway Protocol).
- **Use Case**: Quick, cost-effective setup for secure connectivity.
> **AWS Equivalent**: AWS Site-to-Site VPN + AWS Transit Gateway

### Classic VPN
**Purpose**: Secure, low-volume IPsec VPN connection between on-premises and Google Cloud VPC.
**Features**:
- SLA: 99.9% availability.
- Supports static and dynamic routing (via Cloud Router).
- Uses IKEv1 and IKEv2.
- Does not support client VPN software.
**Use Case**: Site-to-site VPN with basic redundancy.
> **AWS Equivalent**: AWS Site-to-Site VPN

### HA VPN
**Purpose**: High availability VPN with enhanced SLA and redundancy.
**Features**:
- SLA: 99.99% availability.
- Requires 2 or 4 tunnels for SLA compliance.
- Uses BGP for dynamic routing.
- Supports active/active and active/passive configurations.
- Can connect to AWS VPN gateways or other HA VPNs.
- **Use Case**: Mission-critical workloads needing high availability and dynamic routing.
> **AWS Equivalent**: AWS Transit Gateway with VPN
>   - High availability with multiple tunnels.
>   - Supports BGP and ECMP (Equal-Cost Multi-Path) routing.
>   - Can connect multiple VPCs and on-premises networks.

## Cloud Interconnect
![gcp-cloud-interconnect-options](/images/gcp/gcp-cloud-interconnect-options.png)

### Direct Peering
- **What it is**: Connects your on-premises router directly to Google at a Google Point of Presence (PoP).
- **Use Case**: Low-latency access to Google services without traversing the public internet.
- **Limitation**: Not covered by a Google SLA.
> **AWS Equivalent**: AWS Direct Connect Public VIF

### Carrier Peering
- **What it is**: Connects your on-premises network to Google via a partner service provider.
- **Use Case**: When you're not in a Google PoP but want direct access to Google services.
- **Limitation**: No SLA from Google.
> **AWS Equivalent**: AWS Direct Connect via Partner

### Dedicated Interconnect
- **What it is**: Provides private, high-throughput, SLA-backed connectivity between your network and Google.
- **Speeds**: 10 Gbps or 100 Gbps.
- **SLA**: Up to 99.99% if topology meets Google’s requirements.
- **Backup**: Can be paired with Cloud VPN for redundancy.
> **AWS Equivalent**: AWS Direct Connect Dedicated

### Partner Interconnect
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

![gcp-interconnect-options-compared](/images/gcp/gcp-interconnect-options-compared.png)
![gcp-interconnect-decision-tree](/images/gcp/gcp-interconnect-decision-tree.png)