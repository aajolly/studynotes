# DNS, DHCP & IPAM (DDI)
## DNS
![dns_components](/images/ddi/DNS_Components.png)

### DNS Components Explained via a Library Analogy
#### Objective
Resolve the IPv6 address of `eat.example.com` – like finding the shelf number for a book in a vast network of libraries.

**Stub Resolver or Client = You (the reader)**
- You want to read a book called `eat.example.com`.
- But you don’t know where it’s located — so you go ask the front desk librarian.

**Recursive Name Server = Front Desk Librarian**
- This librarian takes your request and promises to find the book for you, no matter how many libraries they must visit.
- They perform all the work on your behalf — this is a recursive query.

**Authoritative Name Servers = The Actual Libraries with Official Records**
- Each library knows about a specific section (zone).
- They answer with authoritative answers like “Yes, I know the shelf location of this book.”

**Zones = Sections of the Library System**
- Each authoritative name server manages a zone — like Fiction, Science, History.
- The zone is a chunk of DNS namespace managed by that server.

**Iterative Query = Librarian Asking Other Libraries**
- If the front desk doesn’t know the answer, they ask other libraries one-by-one:
  - "Do you know where 'example.com' is?"
  - "Can you tell me about 'eat.example.com'?"
- Each library may redirect to another with better knowledge — this is iterative querying.

**Flow Summary:**
1. You (client) ask the librarian (recursive resolver):
    “Where is ‘eat.example.com’?”
2. The librarian searches library-to-library (iterative queries) until they find the right one.
3. Once the authoritative name server (correct library) responds with the exact location (IPv6 address), the librarian gives it to you.

### Common Resource Record Types
#### MX (MAIL EXCHANGE) Record
- Details where to send email for a domain
- With a preference (cost) associated with each target
- Lower number is more preferred
![mx-record1](/images/ddi/dns-mx1.png)

#### SOA (Start of Authority) Record
- Provides administrative & control information for the zone
- Every zone has one
![soa-record](/images/ddi/dns-soa1.png)
![soa-record2](/images/ddi/dns-soa2.png)
- **MNAME**: name of primary name server
- **RNAME**: admin contact email (noc@dns.icann.org) without the `@` symbol
- **SERIAL**: version of zone (common format YYYYMMDDXX)

#### NS (Name Server) Record
- Indicates where the authoritative name servers are for the zone
- Most domains have more than 2 NS records for redundancy
![ns-record1](/images/ddi/dn-ns1.png)

#### SRV (Service) Record
- Provides information on where a service is available
![srv-record1](/images/ddi/dns-srv1.png)

Here's an example of an `srv` record
![srv-record2](/images/ddi/dns-srv2.png)
- **service**: the name of the service, for example, kerberos
- **protocol**: the transport protocol of the desired service, TCP or UDP
- **name**: the domain name where the service is provided
- **priority**: lower value is preferred
- **weight**: used after priority, higher value is preferred
- **port**: TCP or UDP port on which the service is to be found
- **target**: the canonical hostname of the machine providing the service

#### TXT (Text) Record
- Text record can storage any text
- Common uses include forgery detection such as SPF
![txt-record1](/images/ddi/dns-txt1.png)
![txt-record2](/images/ddi/dns-txt2.png)

### Local DNS
![mDNS](/images/ddi/mDNS.png)
![dns-sd](/images/ddi/dns-sd.png)
![dns-troubleshooting](/images/ddi/dns_tshoot.png)
![dns-clear-cache](/images/ddi/dns_clear_cache.png)
![dns-dig](/images/ddi/dns_dig.png)

## DHCP

## IPAM
### Overview
- IP Address Management (IPAM) starts with a simple question:
    - Who has which IP address?
- Network and security administrators often face these challenges:
    - How full iso ur network? Is it time to expand?
    - How many wireless users do we have? Is it time to upgrade?
    - Who has this MAC address? Is it a printer or a desktop? Where is it?

![ipam-history](/images/ddi/ipam-history.png)

#### Spreasheets for IP Address Management?
Major problems with spreadsheets
1. No scalability
    - Multiple sheets with multiple versions lead to conflicts
    - Unintentional deletions happen
2. No auto-updates
    - Admins can't keep up with changing data
    - Without integration or auto-update, IPAM data gets stale quickly
3. No holistic view
    - Creating reports from spreadsheets takes time and labor
    - No historical data

Why is IPAM harder now?
- Mobile devices
- IoT devices
- BYOD
- Virtualization, Containers, Cloud etc

![ipam-metadata](/images/ddi/ipam-metadata.png)
![ipam-metadata2](/images/ddi/ipam-metadata2.png)
![ipam-metadata-tips](/images/ddi/ipam-metadata-tips.png)

#### Role of search, report & alerts in IPAM System
IPAM products needs to support:
- Global database search
- Partial matching
- Metadata search
- Pattern matching

When to use search:
- Before assigning an address
- Tracking down a problematic host
- Investigating an event

Search results need to be exportable or accessible via an API
- Some IPAM products can save search as reports to run regularly

Usage Report
- IPAM database can correlate data and show how `full` a network is
    - How many addresses are assigned
    - How many addresses are available
    - how many hosts are online
- Some IPAM also provide protocol usage report
    - DNS Uusage
    - DHCP usage
- These reports help wth capacity planning

**Data Organization**
![ipam-data-organization](/images/ddi/ipam-data-organization.png)

**Alerting**
- Some IPAM products provide integrated alerts
    - If not, send to a SIEM (System Information & Event Management) product
- When search finds something or report crosses threshold, send alert
    - Email, SNMP trap, syslog, execute a script, or open a ticket via an API
- Example IPAM alerts
    - When available network address is less than 2%, send alert
    - When available DHCP address in range < 5 IPs, send alert
    - When DNS RPZ triggers an action for malicious name, send alert