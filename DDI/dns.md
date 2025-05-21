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

### Less Common Resource Record Types
#### CAA
**Certificate Authority Authorization Record**
- You may publish in DNS information about your certificate authority (CA), so certificate issuers (such as VeriSign) can verify
- When you apply for a TLS/SSL certificate, many certificate authorities are required to check DNS of the domain to see if the request is legitimate or authorized
- Prevents unauthorized certificates from being issues
- Provides mechanism for CA to report violation

#### TSIG
**Transaction Signature Record**
- Provides point-to-point authentication by installing the same key in two places
- Provides message integrity between two parties
- Does not show up in zone file, usually sets up in server configuration

#### NAPTR
**Naming Authority Pointer Record**
- Commonly used in conjunction with SRV records for VoIP
- Allows complex rules to be in place for SIP

#### DNSKEY, DS, RRSIG, NSEC, NSEC3, and NSED3PARAM
- These 6 record types are used by DNSSEC
- DNSSEC is a security extension to make DNS data verifiable and trustworthy
- Stops DNS cache poisoning attacks

#### IPSECKEY, SSHFP, and TLSA
Some record types provide similar out-of-band authentication like TXT record:
![ipseckey](/images/ddi/dns-ipseckey.png)
1. Clients connects to secure resource such as SSH server
2. Secure server sends back security information
3. Client queries DNS for special record type, such as SSHFP
4. DNS server sends back authentication information, client decides to connect or not

#### Vendor-specific Record Types
- **ALIAS**: Used by Infoblox and DNSimple, provides apex (Apex record is the record where FQDN is the same as the zone name, such as example.com) record to CNAME alias.
- **ANAME**: Used by DNS Made Easy, provides apex record to CNAME alias
- **WINS**: like the SOA record, WINS record is attached to the zone's domain name for Microsoft WINS server environment.
- **WINS-R**: provides reverse-mapping for WINS.

### DNS Authoritative Data
- DNS is a decentralized system, not one giant centralized system
- Authority is not kept in one place, it starts at Root, but goes out to corners of the Internet
- Authority is delegated from root to others
    - Think of this as authority being broken down to pieces, and each domain holds their own piece
- That piece of authority, is the authoritative data
    - Usually implemented as authoritative zone

#### Root Name Servers
- Root name servers are authoritative for the root zone (single dot)
- Root servers are a critical part of Internet infrastructure
- Authority starts at root
- When recursive resolvers look up names, they start at root
- Root delegates out authorities so others can maintain their own domain names
    - ROOT -> COM -> EXAMPLE.COM -> ENG.EXAMPLE.COM
- Recursive resolver has the list of root servers, also known as root hints

##### Top-Level Domains (TLD)
- Root hosts all top-level domains (TLD)
- Generic TLD (gTLD)
    - COM, NET, GOV, EDU
- Country Code TLD (ccTLD)
    - AU, CN, IN, JP, RU, SR, US, ZW
- In 2014, registration opened for any top level domain
    - New root zone now contain over a thousand names
    - https://data.iana.org/TLD/tlds-alpha-by-domain.txt

#### Registering Domain Names
- If you want your domain name to be resolvable on the Internet, you'll need to have it registered via a DNS Registrar like Amazon Route53, GoDaddy etc.
- When registering for a domain name, you'll need to provide the name servers and it IPs.
![dns-registrar](/images/ddi/dns-domainRegistrar.png)

#### Authoritative Server
- Authoritative DNS servers provide the definitive data for the zone
- Authoritative DNS servers do not ask others to resolve names in zone
- One zone usually has multiple authoritative servers for redundancy
- Authority comes from the parent zone delegation
    - COM -> EXAMPLE.COM
    - This means, even if I setup my own example.com zone, since the COM servers do not grant me authority, my servers are not authoritative for example.com.

##### Authoritative Server Roles
- **Primary**
    - Holds read-write zone data
    - Source of zone transfer
- **Secondary**
    - Holds read-only zone data
    - Target of zone transfer

##### Common Confusion
- Many people think clients point to primary and secondary DNS servers, they __do not__
- Clients point to preferred and alternate DNS servers, telling clients where to find recursive resolvers
- On UNIX system they are usually set in `/etc/resolv.conf`

#### Name Server Records
- NS records list available authoritative name servers
    - Primary and secondary
- Most domains provide at least 2 NS records for redundancy
- NS records is how the world find your authoritative name servers
- The A / AAAA records associated with NS records are known as glue records

##### Hidden Primary
- Also known as hidden master
- The primary/master server is not listed in NS records
- Reduces load on the primary server
- Primary/master is still visible in SOA record MNAME
- This __does not__ enhance security, it's operational efficiency
- The hidden master could be placed behind a firewall to improve security
![dns-hiddenmaster](/images/ddi/dns-hiddenmaster.png)

#### Zone Transfer
- Standard mathod for authoritative DNS servers to keep zone files in sync on all authoritative name servers
- Think of this as crude data synchronization
- Traditional mode is AXFR (authoritative transfer), where the entire zone is sent every time
- Direction: Primary --> Secondary
- Vendor agnostic, works for most DNS servers (some vendors don't support zone transfers)
- Vendors may have additional replication methods
![dns-axfr-wireshark](/images/ddi/dns-axfr-wireshark.png)

##### Zone Transfer (AXFR)
![dns-axfr2](/images/ddi/dns-axfr2.png)
The secondary server uses the SOA timers to query the primary
SOA Record Revisited
![dns-soa-revisited](/images/ddi/dns-soa-revisited.png)
- **REFRESH**: how often a secondary should contact primary for update
- **RETRY**: how often a secondary should try contacting primary should it fail to reach primary the previous attempt
- **EXPIRE**: how long a secondary can hold on to the zone data when it cannot reach primary
- **MINIMUM**: how long recursive name servers can cache a negative answer such as NXDOMAIN, also known as NCACHE for negative cache.

###### Zone Transfer Considerations
- Zone Transfer must run over TCP (RFC 1035)
- Performance consideration when there are many secondary servers
- SOA timers determine how quickly zone data propagates and how long secondary servers hold on to read-only data

###### Zone Transfer Improvements
- Relying on secondary servers to check-in is not very efficient
- RFC 1995 adds IXFR (Incremental Zone Transfer)
- RDS 1996 adds NOTIFY
- Primary can notify secondary when changes occur
- Primary sends only delta instead of the entire zone
- This is less resource intensive, and secondary servers get updated data much faster
Example Zone Transfer using NOTIFY & IXFR
![dns-notify-ixfr](/images/ddi/dns-notify-ifxr.png)

##### Vendor Data Storage and Synchronization
- BIND
    - Default text file, can be configured to use database such as MySQL
- Microsoft
    - AD-integrated zones are stored in LDAP database
    - Synchronization using built-in AD-replication on interval
- Infoblox
    - DNS data storage in proprietary database
    - Use encrypted VPN tunnel for database synchronization
- tinydns (djbdns)
    - Uses rsync over SSH for data synchronization
    - Does not support zone transfer

### Zones and Delegation
#### Authoritative Zones
- Authoritative zone holds authoritative data
- Zones reflect organizational logic
- Authoritative zone is present on both primary and secondary servers
    - Enables answering authoritative data without asking others
- Two types of authoritative zones:
    - Forward-mapping zone (name to address)
    ![dns-fwd-zone-mapping](/images/ddi/dns-fwd-zone-mapping.png)
    - Reverse-mapping zone (address to name)
- Zone minimum requirement
    - Zone name
    - SOA record
    - NS record

#### Reverse Mapping Zone
- Reverse-mapping zones are directly tied to network sizes
- For IPv4 reverse, /8, 16, and /24 are common and easy to setup
- If there are multiple /24 networks, consider setting up a single reverse zone for the /16
    - For example, numerous 192.168.x.x networks can be one single reverse-mapping zone 168.192.in-addr.arpa
- Choice depends on size of network, number of DNS zones, and environment, there's no `right` answer.
![dns-reverse-zone-mapping](/images/ddi/dns-reverse-zone-mapping1.png)
![dns-reverse-zone-mapping2](/images/ddi/dns-reverse-zone-mapping2.png)
![dns-reverse-zone-mapping3](/images/ddi/dns-reverse-zone-mapping3.png)
![dns-reverse-zone-mapping4](/images/ddi/dns-reverse-zone-mapping4.png)

#### Zones and Delegations
##### Subzone
- Also known as `child zone`
- A zone inside of a zone
    - For example, `example.com` can have a sub zone `baby.example.com`
- Allows flexibility within organization to manage data for subdivisions
- Parent zone administrator controls who gets to administer the child zone
- Child zone data is hosted alongside parent zone data
##### Delegation
- Delegation is letting subzone administer itself
- Delegated zone has full control of its own servers and authoritative data
- Delegation is what glues the global DNS name space together
    - Root delegates to `com`
    - `com` delegates to `example.com`

##### Glue records
- Glue records are A/AAAA records for the NS records
- Glue records are critical to delegation, they connect, or glue, parent to child
- Incorrect glue records cause lame delegation
![dns-delegation](/images/ddi/dns-delegation.png)

##### Comparing Subzone and Delegation
| Subzone | Delegation |
| :-----: | :------: |
| Parent hosts child's data | Child needs to host own data |
| Parent maintains full control | Child has full administrative control |
| Child data can be on same server | Child data must be on other servers |
| | Parent sends referral for queries in child |

#### Lame Delegation
- Caused due to human error
- It occurs when the parent zone has incorrect NS and glue records about the delegated child zone
    - Parent zone could delegate to incorrect IP addresses that do not have DNS service running
    - Parent zone could delegate to wrong DNS server, who refer back to root
- Lame delegation symptoms:
    - Unable to resolve delegated child domain name
    - Slow resolution as recursive resolvers timeout on invalid addresses
    - Delegated names sometimes resolve, sometimes do not
![dns-lame-delegation1](/images/ddi/dns-lame-delegation1.png)
![dns-lame-delegation2](/images/ddi/dns-lame-delegation2.png)

#### Reverse Mapping Delegation
- IPv4 reverse-mapping delegation was designed with classful in mind
    - Class A, B, and C
    - Impossible to do classless delegation
- RFC 2317 allows IPv4 classless delegation such as /27
- IPv6 reverse doesn't have this problem
- This means DNS and network address assignment are linked, poor network address assignment directly impacts reverse-mapping

### Query & Caching
#### DNS Header
![dns-header-format](/images/ddi/dns-header-format.png)
![dns-header-example](/images/ddi/dns-header-example.png)
##### MESSAGE ID
- Also known as Transaction ID, TXID, or Query ID
- 16 bits for 65,536 possible ID's
- Server must copy Query ID from query to response
- Resolver ignores response if Query ID's do not match

##### RESPONSE CODES
| Code | Name | Description |
| :---: | :---: | :---: |
| 0 | NOERROR | No error |
| 1 | FORMERR | Format error, usually indicates client sending malformed query |
| 2 | SERVFAIL | Server failure, generic error message indicating error occured on server side |
| 3 | NXDOMAIN | Non-existent domain, indicates the domain name queried does not exist |
| 5 | REFUSED | Server configuration does not allow it to response to client |

##### HEADER FLAGS
| Flag | Name | Set to 0 | Set to 1 |
| :---: | :---: | :---: | :---: |
| qr | query response | this message is a question | this message is an answer |
| aa | authoritative answer | not authoritative | authoritative |
| tc | truncation | message not truncated | message truncated, retry on TCP |
| rd | recursion desired | iterative query | recursive query |
| ra | recursion available | recursion not available | recursion is available |
| ad | authenticated data | response not authenticated | response is authenticated |

__**NOTE**__
- `ad` flag is used by DNSSEC

#### Authoritative and Non Authoritative Answers
![dns-client-server](/images/ddi/dns-client-server.png)
![dns-server-2-server](/images/ddi/dns-server-2-server.png)

#### Caching and TTL
- Recursive resolvers can only cache answers according to TTL
- Authoritative servers decide TTL value
- Stub resolvers such as web browsers do not observe TTL
    - Typical browsers caches DNS entries for 30 seconds
- Recursive resolver can choose to override TTL
    - Some ISP's override low TTL (ex. 10 seconds) with higher TTL (ex. 1 hr)
##### TTL Considerations
- Low TTL
    - Higher load on name servers
    - Changes propagate faster
- High TTL
    - Less load on name servers
    - Changes propagate slower

### Forwarding and Query Path
#### Forwarding
- Forwarding is the act of DNS server sending (forwaring) a recursive query to another DNS server
- By definition, only recursive resolvers can perform forwaring
- If no default forwarders are defined, the recursive resolver goes to root to resolve all unknown domains
- Default forwaring is sometimes called `global forwarding`, because it impacts the direction of every query on the DNS server (think of default router in Networking)

##### Forward-Only
- Some vendors call it `use forwarders only` or some varition
- By configuring default forwarders, the recursive resolver is sending all queries to the default forwarders instead of root
- What if the default forwarders are down?
    - `forward-only` option controls whether or not the recursive resolver falls back to using root
- There is no right or wrong, it depends on the environment and the desired behavior

![dns-forward-only](/images/ddi/dns-forward-only.png)
- In the example above, the blue DNS server is client facing and is configured as forward-only. The red DNS server is the default forwarder.
- If the red DNS server becomes unavailable, the blue client will not forward the query to the root.
![dns-forward-only-example](/images/ddi/dns-forward-only-example.png)

![dns-default-forwarding](/images/ddi/dns-default-forwarding.png)

#### Conditional Forwarding
- Conditional forwarding allows recursive resolvers to forward specific domain names to specific targets
- DNS administrators can use conditional forwarding to control flow of DNS queries
- Common use is for internal domains such as `partner.local` that can cannot be resolver by quering root
    - Likely receive NXDOMAIN from root
- Most DNS server products allow conditional forwarding by creating a forward zone
![dns-conditional-forwarding](/images/ddi/dns-conditional-forwarding.png)
![dns-conditional-forwarding-example](/images/ddi/dns-conditional-forwarding-example.png)

#### Delegation vs Forwarding
| Delegation | Forwarding |
| :----: | :----: |
| Delegation is only from authoritative to authoritative server (think parent to child) | Forwarding can be from recursive to other recursive resolver |
| Stub resolvers/clients cannot follow delegation | Forwarding can also be from recursive to authoritative server |

##### Load Balancer: Delegate or Forward?
- Scenario:
    - We want the name `fast.example.com` to be served by GSLB (Global Server Load Balancing)
    - GSLB will look at the incoming DNS query's geolocation and return the closes DNS answer
    - For example, query originating in Japan, `fast.example.com` = Asia IP
- DNS administrators are asked to configure authoritative DNS server to work with load balancer
- Must decide between delegation or forwarding? Lets see some examples
![dns-delegation-gslb-example1](/images/ddi/dns-delegation-gslb-example1.png)
Following the example above, here's another example where the query is being generated from Taipei
![dns-delegation-gslb-example2](/images/ddi/dns-delegation-gslb-example2.png)
Now lets examine the same example from above but with forwarding
![dns-forwarding-gslb-example1](/images/ddi/dns-forwarding-gslb-example1.png)
> __**NOTE**__: As a best practice internet facing authoritative name servers should not have recursion enabled.

The conditional forwarding will result in a REFUSED response.
What if it is enabled for recursion?
The GSLB will view the request coming from Paris and it'll respond with the Paris IP instead of Taipei as shown below.
![dns-forwarding-gslb-example2](/images/ddi/dns-forwarding-gslb-example2.png)

#### Advanced Query Path
- Determining the correct query path should be the first step in DNS troubleshooting
- Otherwise, you risk wasting time troubleshooting the wrong server or drawing the wrong conclusion
- DNS forwarding is a per-hop behaviour, unlike network traceroute, meaning you have to check each DNS server and zone configuration along the way

### ACLs and DNS Views
#### Access Control List (ACL)
Simple ACL example in BIND:
```bash
options {
    allow-query { 10.0.0.0/8; 172.16.0.0/12; 192.168.0.0/16; };
};
```
- Allowing All RFC1918 to query this server
![dns-acl-ordering](/images/ddi/dns-acl-ordering.png)

#### DNS Views
- A DNS view is a set of zones with an ACL
- The purpose is to allow DNS servers to provide different answers based on the source of the query
- Most organizations have two or three views
- Sometimes called `split DNS`, `split horizon`, or `split-brain DNS`
![dns-view](/images/ddi/dns-view.png)
> __**NOTE**__
> - Once client matches on a DNS view, it receives answer from that view
> - There is no "try another view" option
> - For example, client queries for **lab.example.com** in Internal view and the answer is NXDOMAIN, that is authoritative
    - DNS server get **lab.example.com** out of a different view for the same client

### Local DNS
![mDNS](/images/ddi/mDNS.png)
![dns-sd](/images/ddi/dns-sd.png)
![dns-troubleshooting](/images/ddi/dns_tshoot.png)
![dns-clear-cache](/images/ddi/dns_clear_cache.png)
![dns-dig](/images/ddi/dns_dig.png)

### DNS Troubleshooting Tips
Once you have confirmed the issue is rooted in DNS, approach DNS troubleshooting in this order:
1. Lookup tools like dig
2. Query path
    - Figure out how queries are being passed before further analysis
    - Query path tells us where to go next
    - There's no easy way to trace, you may need to log in to each DNS server to learn the true path
3. Logs
    - DNS server keeps different levels of logs
    - Consider enabling query logging during troubleshooting
        - But disable it when finished, query logging could impact performance
    - Send logs to centralized server for archive
        - Run log analyzer in one place
        - Keep historical data
4. Packet capture
    - Last resort for DNS troubleshooting
    - DNS header tells us a lot, usually do not need to go this deep
    - If a query goes unanswered, there's no logs or DNS message to analyze, may need packet capture to see if it's a network issue

### DNS Network Considerations
#### DNS and Network Assignment
- Keep DNS reverse-mapping in mind when allocating addresses
- If possible, assign in CIDR blocks to organizations, such as entire /24
- This has direct impact on network efficiency such as routing tables
- Also makes reverse-mapping DNS easier to manage

#### Network Considerations along query path
This section is related to larger DNS messages as they traverse through network:
- **EDNS0**
    - Traditional DNS UDP messages are limited to 512 bytes
    - Extension mechanism for DNS version 0 (EDNS0)
        - EDNS0 allows larger size up to 4096 bytes (1220 bytes recommended)
    - New feature such as DNSSEC requires EDNS0
    - Per-hop configuration (each server must support EDNS0)
    - If disables, causes subtle issues in DNS where larger responses may not get through, or switch to TCP
    - Post 2019, EDNS0 is mandatory, ENDS0 is defined in RFC 6891
- **TCP**
    - DNS has always required the use of TCP/53
    - TCP is required for zone transfers
    - As fallback mechanism when UDP cannot fit message in a single packet
    - Modern DNS messages are getting bigger:
        - Large RRset with AAAA records
        - TXT records
        - New records (DNSKEY and RRSIG)
    - Inconsistent DNS resolution if TCP/53 is blocked, some answers get though, some don't
- **MTU and MSS Size**
    - Maximum Transmission Units (MTU) dictates size of data chunks, should IP fragmentation occur
    - Large data size requires breaking down to small chunks of MTU-size, and re-assembled on the far end
    - Configured per network
    - If mis-configured, large DNS responses may be lost, not assembled correctly on the far end, or requires frequent retransmission

### DNS Split Authority
- Common issue experienced by corporate users
    - When I'm in building A, I can resolve name `x.example.com`, but not when I'm in building B
- This is most likely due to DNS split authority or DNS views
- This may be intentional, and probably implemented using DNS views
- If this is not intentional, it is often caused by zone files out of synchronization
    - More common in timed-synchronization such as AD-replication
    - Or two separately maintained authoritative name servers not integrated
- Split DNS can become very complex, especially when dealing with reverse-mapping zones
- In many instances, forward lookup may work, but reverse-lookup fails
- Many applications rely on successfully forward and reverse lookup, and if reverse lookup fails, application may fail in subtle ways
    - Ex: RDP may not connect to bob-laptop.foo.com if reverse lookup fails

## DNS Anycast
- Anycast puts the same IP address on multiple servers
- Let routing take user to the nearest available node
- Not unique to DNS protocol, but most commonly implemented on DNS
- Examples:
    - CloudFlare DNS: 1.1.1.1
    - Google DNS: 8.8.8.8

| Advantages | Disadvantges |
| :----- | :----- |
| Increased reliability | Increased complexity |
| Load desitribution | Difficult to troubleshoot |
| Improved performance | Routing issues may impact DNS |
| Simplified client configuration | |
| Resilience against volumetric attacks | |

## DNS Security Considerations
- There are many security threats that are DNS-related
- DNS security is of much higher concern than DHCP security
    - DHCP security is localized, DNS security is global
- Here's a small subset of DNS security threats

### Impersonator attack
- Attacker can register lookalike names that look very similar to legitimate domain names
- Includes attacks such as homograph attack
- IDN makes this even harder to detect with internationalized characters
- Some attackers use web browser tricks to hide the URL bar
- Examples:
    - PayPa`L`.com and PayPa`I`.com
    - g`oo`gle.com and g`oo`gle.com (uses Greek leterr omicron, punycode xn--ggle-Onda.com)
### DNS Tunneling and Malware
- DNS Tunneling: Technique used for sending other data over DNS
- Malware uses DNS tunneling for command & control (C&C)
![dns-tunneling](/images/ddi/dns-tunneling.png)

### Data Exfiltration
- Similar tunneling can be used to exfiltrate data through DNS
- Response data is not important in exfiltration
![dns-data-exfiltration](/images/ddi/dns-data-exfiltration.png)

#### DNS Threat Intelligence
- Impersonator attacks, tunneling, and data exfiltration all rely on attacker's domains
- DNS Threat Intelligence is a collection of databases of `bad` domains
- Security researchers work hard to keep this information up to date
- Recursive resolvers can be armed with this information so they don't return malicious domain names
- The mechanism to download database to recursive resolver is RPZ

#### Response Policy Zone (RPZ)
- RPZ, is the standard for synchronizing DNS security policy data via DNS zone transfer (TCP/53)
- Recursive resolver can log and deny queries for malicious domains
![dns-rpz](/images/ddi/dns-rpz.png)
> __**NOTE**__: The above example is not effective against data exfiltration, additional configuration is needed

### Amplification and Reflection
- **Amplification** is when a small query can result in a large response
- **Reflection** is when attacker spoofs IP of victim and redirects traffic
- Attacker sends small queries to recursive resolvers, generating large load that knocks victim offline
- These are ACL and rate-limiting techniques that can mitigate this attack
![dns-amplification](/images/ddi/dns-amplification.png)

### Cache Poisoning and DNSSEC
- **Cache poisoning** is the general technique of attacker tricking recursive resolver to store malicious answers in cache
- Very difficult for user to detect that they are given incorrect answers
- This problem is rooted in the lack of authentication in DNS
    - This is no solution except for a protocol update
- DNSSEC is the protocol extension that adds authentication to DNS
    - Secures communication among DNS servers

### mDNS Security Considerations
- mDNS is UDP-based multicast communication, easy to spoof
- If attacker gets access to local subnet, it can easily return false answers
- Malicious client can launch amplication and reflection attack against other mDNS client
- If exposed to Internet, could leak private information meant for local use only