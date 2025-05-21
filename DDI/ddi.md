# DNS, DHCP & IPAM (DDI)
# Redundancy and Reliability
## Fault Tolerance
### Importance of Fault Tolerance
- Both DNS and DHCP are critical services, they need to be reliable. However, systems will experience down time and maintenance
- Architecture needs to be designed with fault tolerance in mind
- DNS and DHCP have some built-in fault tolerance to the original protocols

### Built-in Fault Tolerance
#### DHCP
- If DHCPDISCOVER or DHCPREQUEST go unanswered, client will resend
- Client holds on to address, doesn't lose address the moment server goes offline
- DHCP administrator can use long lease time on networks that need DHCP service better fault tolerance. This is very client-dependent

#### DNS
- Authoritative DNS Server
    - At least 2 NS records for internt-facing authoritative zones
    - Root server A/AAAA records have a TTL = 3600000 seconds (42 days)
- Recursive DNS server
    - Client tries preferred server first, falls back to alternate servers
    - Client app such as browser will retry
    - Client-side caching

### System-Level Fault Tolerance
- If vendor supports it, use system-level redundancy
- Many protocols allow increased fault tolerance by putting up two or more systems to maintain one virtual IP:
    - HSRP
    - VRRP
- Think system redundancy, not protocol
    - No client timeout

### Network-Level Fault Tolerance
- If the vendor supports it, use network interface (NIC) level redundancy
- Also known as bonding or network redundancy
- Connect two network interfaces from system to network switches for increased fault tolerance
    - One link goes down, the other link remains up
- Options:
    - EtherChannel(PAGP)
    - IEEE 802.3ad (LACP)

## Load Balancing
### Load Balancing DHCP
- DHCP server load generally is not high, so no need for load balancing
    - Some use load balancer to increase DHCP fault tolerance
- Using load balancer with DHCP is not recommended
    - DHCP servers do not have a way to synchronize lease database
    - Two DHCP server not in sync create conflicts
- Use DHCP Failover protocol instead

### Load Balancing DNS
- DNS has built-in data synchronization (zone transfer)
    - Easier to do load balancing than DHCP
- Authoritative DNS needs this less, since you can list multiple NS records and rely on round-robin
- Recursive server could benefit more, since client timeout and retry is not reliable
- Even better option is to use DNS Anycast

# Metadata Design
## Logical Metadata Format
- Most IPAM products lets users create custom metadata fields
- Choose data format that best represents the metadata you want to track
    - Activation Date -> data selector
    - Fixed choices -> drop-down list
- Avoid free text field to reduce entry errors
- If IPAM product supports object inheritance, it can reduce manual entry
    - Child objects inherits metadata from parent

## Designing Logical Metadata
- Don't just create new logical metadata fields
- Poorly thought out metadata fields leads to less useful IPAM data
- Really poor design frustrates users, who will type in bogus data

### Logical Metadata Design Flow
Start with the below 5 questions:
1. What information do I want to track?
    - What would help me organize data better?
2. What objects do I want to track this information on?
    - Track information for DNS records only? Subnets?
3. What data format should it be?
    - Numeric? Drop-down list? Text field?
4. Allow multiple?
    - One-to-one mapping? Or one-to-many?
5. Should this metadata be optional or required?
    - Optional? Recommended? Mandatory?

Below are some examples
**Example 1**:
| Question | Example | Description |
| :----- | :----- | :----- |
| What information do I want to track? | City | |
| What objects do I want to track this information on? | Subnets | |
| What data format should it be? | Drop-down list | |
| Allow multiple? | Yes, one subnet can be in multiple cities | |
| Should this metadata be optional or required? | Optional | |

**Example 2**:
This example is based on a scenario that a ticketing system is in use for requesting information and the network manager would like to track information based on request ID.

| Question | Example | Description |
| :----- | :----- | :----- |
| What information do I want to track? | Request ID | Request ID from the ticketing system |
| What objects do I want to track this information on? | DNS records, DHCP ranges, manual allocations | Track all sorts of objects |
| What data format should it be? | Alphanumeric | Ticketing systems usually use an alphanumeric data format for ticket numbers or requests |
| Allow multiple? | No, one request ID per object creation | You'd want to track information on a per request ID basis |
| Should this metadata be optional or required? | Mandatory | In order to follow the process, this field should be mandatory |

## Metadata Design Tips
- Naming is important
    - If a metadata field has an ambiguous name, users don't know what it does
    - Bad examples: CRID, Site, MyAwesomeCompany_Internal_Asset_Number
    - Good examples: Change Request ID, Airport Code, Asset #
- Restrict user entry
    - Bad examples: "New York, NY", "NYC", "nyc", and "New York City"
- One field, one data
    - Don't have a field called "Name and Address"
- Optional vs Mandatory
    - Too many mandatory fields may result in user typing in bogus info
    - Mandatory requirement might break current process
    - Consider let automation process enforce what is optional and what is mandatory
- Use open standards such as ISO wherever possible

# API and Automation
- Application Programming Interface (API) allows users to interact with a system via defined software calls
    - Administrators can automate
    - Engineers can integrate
- API is the system's own language
- You speak the language of the system, you control the system!

## Limitations of API
- Think of API as the elevator buttons, you can only access what the designer lets you access.
![elevator](/images/ddi/elevator.png)
- Example: If the vendor doesn't allow you to delete an address, then you cannot delete the address

## What to look for in an API
- Community support
    - Large and active community provides peer support
- Features
    - Has the features you want or need
- Application and vendor support
    - Integration with the applications and products you want
- Documentation
    - Poor documentation usually indicates poor support, goood documentation saves time

## Automation and Process
- Benefits of automation:
    - More efficient
    - Less human error
    - Consistent process
- Process is more important than automation
    - Automation = how
    - Process = why
- API is the key to automation:
    - Define the current manual process
    - Find high priority pieces to be automated
    - Use API to create automation

### Examples
![automation-example1](/images/ddi/automation-example1.png)
![automation-example2](/images/ddi/automation-example2.png)