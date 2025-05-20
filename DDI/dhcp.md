# DHCP
## DHCP Messages
#### DHCPv4 Message Format
![dhcp-message-format](/images/ddi/dhcp-message-format.png)
The DHCP protocol evolved from the earlier BOOTP Protocol and uses its flags. The wireshark capture below shows an example
![dhcp-wireshark](/images/ddi/dhcp-wireshark.png)
- **ciaddr**: Client Address
- **yiaddr**: Client Address assigned by DHCP Server
- **siaddr**: Next server IP Address
- **giaddr**: Relay agent (Router) IP address
- **chaddr**: Client MAC address

#### DHCP DORA
Discover -> Offer -> Request -> Ack

##### DHCP Discover Message
![dhcp-discover1](/images/ddi/dhcp-discover1.png)
`src` is set to 0.0.0.0 as the Client doesn't have an IP address yet. It sends a broadcast message (255.255.255.255) on UDP/67. Typical implementations have a DHCP relay like below.
![dhcp-discover2](/images/ddi/dhcp-discover2.png)
The router(relay agent) updates the `giaddr` flag to reflect its own IP. This is used by the DHCP server to identify the pool to use. See below example
![dhcp-discover3](/images/ddi/dhcp-discover3.png)

##### DHCP Offer Message
The DHCP server looks at the `giaddr` flag and assigns `10.1.1.5` address and updates the `yiaddr` flag.
![dhcp-offer](/images/ddi/dhcp-offer.png)

##### DHCP Request Message
![dhcp-request](/images/ddi/dhcp-request.png)

##### DHCP ACK Message
![dhcp-ack](/images/ddi/dhcp-ack.png)

##### DHCP Release Message
![dhcp-release](/images/ddi/dhcp-release.png)

##### DHCP NAK Message
- DHCP server can run as authoritative or non-authoritative
- Non-authoritative is there to allow DHCP servers on the same network, maintained by differet administrators
- If authoritative, it can send DHCPNAK to tell client to not only deny its request, but to indicate it's the `wrong` thing to ask
- Common reasons:
    - Client requests IP for wrong subnet (packet from 10.x.x.x gateway but requesting 192.x.x.x)
    - REQUEST for expired lease
    - REQUEST without DISCOVER

Here's an example of DHCPNAK
![dhcp-nak](/images/ddi/dhcp-nak.png)

## DHCP Objects
DHCP specification doesn't include these objects, these are constructs implemented by various vendors. Some of the commonly used objects are listed below.
### Network 
This is the container object that houses other objects. Example: 10.0.0.0/24

### Range
This is also called the scope and usually refers to the IP range that DHCP is allowed to use to allocate IP address. Example: 10.0.0.11 - 10.0.0.20

### Exclusion
As name suggests is an exclusion list of addresses. Example: 10.0.0.14 - 10.0.0.16

### Manual Allocation
Manually allocate an IP address to device. Example: 00:00:de:ad:be:ef -> 10.0.0.25. DHCP Server tries to allocate the same address to the same client, however it is not guaranteed. Hence, manual allocation. Common use cases for manual allocation:
1. Devices that provide network services like printers, scanners, WiFi access points etc
2. Devices that need to be tracked by the same address every time like CEO's computer
3. Devices that need to be guaranteed an address from DHCP

### Policy
The algorithm used by the DHCP server to determine which client receives what IP address. This is not governed by standards, but different vendors have different implementations. Here's an example where DHCP server is configured to assign IP based on device type.
![dhcp-policy1](/images/ddi/dhcp-policy1.png)

While there's no standard for DHCP policy processing, most vendors implement something like below.
![dhcp-policy2](/images/ddi/dhcp-policy2.png)

### Deployment Recommendations
#### DHCP Server Recommendations
1. Single authoritative DHCP server to avoid conflicts
    - Multiple DHCP servers for one subnet is not recommended
2. Best practice is to avoid IPv4 split scope
3. For DHCPv4 redundancy, consider DHCP Failover

#### Network Recommendations
1. DHCP service should be enabled on a subnet-by-subnet basis
    - Access concerns
    - Address conflicts
2. Typically, networks with these devices need DHCP:
    - Voice over IP phones
    - Wireless devices
    - IoT devices
3. These networks should not have DHCP:
    - Subnets where most hosts have manually configured addresses (Static IP)
    - Large size subnets (example: /18)

#### Range Recommendations
1. Multiple ranges in a network is possible
2. Place different classes of devices in different ranges with the help of DHCP Policy
    - Example: printers get higher addresses, dekstops get lower addresses
> __**Note**__ This is not true separation or quarantine because all ranges are still in the subnet

3. Alternatively, use subnetting to separate devices
    - Example: 192.168.1.64/26 for desktops, 192.168.1.128/27 for printers

#### Exclusion Recommendations
1. If vendor does not support exclusion, you have to use two ranges
2. Must have a way of annotating the excluded addresses as not available. Otherwise people may think those blank IP addresses are free and can be used for something else.

#### Manual Allocation Recommendations
1. Benefits of DHCP manual allocation over static IP address assignment:
    - **Scalability**: easier to deploy through DHCP
    - **Configuration**: DHCP can deliver more than address, such as DHCP options
    - **Manageability**: changes can propagate automatically using DHCP
    - **Requirement**: some devices may only support DHCP
2. Possible workflow: create manual allocation before deployment
    - When printer powers up, device automatically gets the address assigned

## DHCP Timers and Lease States
### DHCP Lease
#### DHCP Lease Ordering
Most DHCP servers lease out IP addresses in this order:
1. DHCP policy (allow/deny)
2. Manual allocation (DHCP host)
    - Server has explicit information configured for the device
    - Client receives the same IP address every time on the same network
    - Client still needs to renew
2. Dynamic allocation
    - Server chooses from a range or pool
    - There's no guarantee client gets the same IP address every time

#### Address Assignment
- Server chooses address to give out
    - Some start from high address, some start from low, some random
- Some servers send ICMP to see if the address is already in use to avoid collision or address conflict
    - This behaviour is configurable
- Server makes best-effort attempt to assign client the same address if possible, but no guarantee

#### Lease Duration
- DHCP was designed to re-use IP addresses after a while to avoid address exhuastion
- Each client is given a limited time period to lease the IP address
- Permanant lease is not a good idea
    - Leads to IPv4 address exhaustion
    - Client does not check in to receive updates
    - Usually indicative of poor client implementation

### DHCP Timers and Lease States
#### DHCP Lease Time and Timers
- IP address is leased to the client for a set amount of time (lease time)
    - Option 51: lease time
    - Client is responsible for renewing lease to continue using the address
- Server decides when client renews lease with timer settings
- T1 Timer (Renewal, option 58)
    - Client unicast to DHCP server with DHCPREQUEST to extend lease
- T2 Timer (rebinding, option 59)
    - Client unable to contact original DHCP server
    - Broadcast DHCPREQUEST and hope someone responds

##### T1: Renew Timer
![dhcp-t1-timer](/images/ddi/dhcp-t1-timer.png)

##### T2: Rebind Timer
![dhcp-t2-timer](/images/ddi/dhcp-t2-timer.png)
![dhcp-t2-timer2](/images/ddi/dhcp-t2-timer2.png)
![dhcp-server-unavailable1](/images/ddi/dhcp-server-unavailable1.png)
![dhcp-server-unavailable2](/images/ddi/dhcp-server-unavailable2.png)
![dhcp-server-unavailable3](/images/ddi/dhcp-server-unavailable3.png)

#### Lease Time Design Consideration
| Short Lease Time | Long Lease Time |
| :------ | :------ |
| Changes picked up faster by clients | Changes take longer to propagate |
| Less waste of address space as leases expire faster | Need larger address space as leases take longer to expire |
| Higher load for both server and client | Less load for both server and client |
| Outage of DHCP server more visible to clients | Outage of DHCP server less visible to clients |
| Example: guest wireless network | Example: remote office network |

### DHCPv6 Lease Lifetime and Timers
#### DHCPv6 Lease Life Time
- DHCPv6 has a concept of gracefully unbinding an address through two phases: preferred and deprecated
- During normal operation (preferred), client can make any connections
#### DHCPv6 Preferred Life Time
- If the client did not renew lease by the time the Preferred Life Time has run out, it enters deprecated state, where it can keep existing connections, but cannot make new ones.

#### DHCPv6 Valid Lease Time
- If the client still did not renew at the end of the Valid Life Time, the client is really gone, the lease is in the invalid state
- Valid Life Time must be greater than Preferred Life Time

![dhcpv6-lease-timers](/images/ddi/dhcpv6-lease-timers.png)

#### DHCPv6 Timers
- T1 and T2 timers are calculated off of Preferred Life Time
- T1 (Renew) is typically around 50%
- T2 (Rebind) is typically around 80%

![dhcpv6-timers](/images/ddi/dhcpv6-timers.png)

### Address Binding States
#### IP Address Binding States
DHCP server keeps these states for IP addresses:
- **Free**
    - The address is available for allocation
    - Leases start in this state
- **Active**
    - The address is in active use by a client
    - As long as no other signals otherwise, lease stays active
- **Expired**
    - Lease time has passed, client is assumed to be not using address
    - Many DHCP servers will keep lease as EXPIRED for a while instead of moving it to FREE right away, in case clients come back.
- **Released**
    - Address has been actively released by client
    - Less common
    - Many DHCP servers keep lease as RELEASED for a while instead of moving it to FREE right away, in case clients come back and want the same IP
- **Reset**
    - Address has been released by administrator
    - No way for server to signal to client
    - Client does not lose IP address
    - Used when administrator needs to move ranges
- **Abandoned**
    - When server pinged address before leasing out, address responded, indicating someone is using address without knowledge of DHCP server
    - DHCP server will avoid giving out this address whenever possible
    - However, if DHCP server is out of other addresses, it may give away an ABANDONED address, resulting in IP address conflict
    - ABANDONED indicates a network problem waiting to happen, should be investigated proactively
    - Entirely depends on ICMP for detection (though most modern networks don't allow ICMP)
- (Backup) - Only used during DHCP failover
![dhcp-address-binding-states](/images/ddi/dhcp-address-binding-states.png)

## DHCP Options
### DHCP Options
DHCP Options are mandatory for a successful DHCP implementation and are defined in RFC2132. 

#### Options for DHCP Operation
- **Option 50**: Address Request
- **Option 51**: Lease Time
- **Option 53**: DHCP Message type (ex: DHCPDISCOVER)
- **Option 54**: DHCP Server Identifier (siaddr)
- **Option 55**: Parameter Request List (PRL)
    - Clients muct ask server for the list of options in Option 55
    - It is a sequence of numbers, each number is the DHCP option client is requesting
        - Example: 1,3,6,15,150,42
    - Most DHCP servers only returns what the client asks (RFC 2132 does not explicitly specify this server response, different DHCP servers may respond differently)
        - Some DHCP servers allow sending back more options than requested
    - As a client, you always need to request for what you need, don't depend on the server sending you extra options.
- **Option 58**: Renewal Time (T1)
- **Option 59**: Rebinding Time (T2)

#### Common Options
- **Option 3**: Router (Default Gateway)
- **Option 6**: DNS Servers
- **Option 12**: Hostname
- **Option 15**: Domain Name
- **Option 42**: NTP Servers
- **Option 60**: Vendor Class Identifier

#### DHCP Option in Message
![dhcp-option-detail](/images/ddi/dhcp-option-detail.png)

#### Setting Options at different levels
- Some options are better set at the higher level to simplify configuration
- Some options need to be set at lower level due to uniqueness (ex: router)
- Configure common or default option values at highest level possible
- Lease time example:
    - Server-level: default lease time 48 hours
    - Network-level: warehouse network lease time 6 hours
    - Range-level: printer range in warehouse network 120 hours
![dhcp-option-example](/images/ddi/dhcp-option-example.png)

### Undefined Options
- Sometimes the DHCP server may not have the option the client is looking for
- Administrator can create a new definition based on the documentation and client needs:
    - Option code (ex: 71)
    - Option data type (ex: ip-address)
    - Option name (ex: NNTP)
- Once the new option has been created, it can be used like any standard option
    - can be defined at any level
- Client still needs to request for the option in PRL
![dhcp-undefined-option](/images/ddi/dhcp-undefined-option.png)

### Vendor Specific Options
- Sometimes, equipment may require special parameters that is not part of the standard DHCP options
- Vendor usually provides instructions on the codes needed, and these codes will be added and delivered to the equipment using Option 43
- While configuring this form the group up is rare, Option 43 is common for networks that have VoIP and wireless equipment
![dhcp-option43](/images/ddi/dhcp-option43.png)

#### Sending Option43
- DHCP policy is required to send Option 43 to the right clients
- Client sends many DHCP options that could identify itself
    - Such as vendor class identifier (option 60) or hostname (option 12)
    - Option 60 usually contains vendor name or make-and-model
    - DHCP server can use this information to identify specific devices
- DHCP server uses policy to identify client
    - When match found, server sends option 43
    - Option 43 contains vendor codes and value

![dhcp-option43-example](/images/ddi/dhcp-option43-example.png)

### DHCPv6 Options
- Works the same as DHCPv4
    - Client requests for a list of options, server returns them
- Different numbers in the DHCPv6 option space
    - DNS is no longer 6, it's 23
    - NTP is no longer 42, it's 56
    - No default gateway option (was Option 3 in DHCPv4)

## Dynamic DNS
### DDNS Update Process
![ddns-update-process](/images/ddi/ddns-update-process.png)

### DHCP Option 81
- Rather than taking the domain name (option 15) from DHCP server, DHCP clients can send FQDN (option 81), and ask DHCP server to update a different DNS zone
- DHCP server must be configured ahead of time to support option 81
- Client FQDN option is specified in RFC 4702

![dhcp-option81-example](/images/ddi/dhcp-option81-example.png)

### DDNS Considerations
#### Accuracy
- Worst thing that can happen to DNS is inaccurate data
- Clients are terrible at cleaning up after themeselves
    - Unable to reach back to the network to remove its own DNS records
- DNS data left behind becomes stale and inaccurate, resulting in timeout for end users (connecting to IP addresses that are offline)
- If DHCP server performed DDNS updates, it can clean up DNS as client leases expire, keeping DNS data more accurate
- DNS server scavenging process is not very reliable

#### Update Protection
- Traditional DNS does not distinguish between records created manually and records created dynamically
- Typically, manually created records are more important, and should be protected from being overwritten dynamically
    - Example: `www.example.com` gets overwritten by a client named `www`
    - This can lead to security issues or unreliable DNS service
- Some DNS vendors provide proprietary protection measures
- RFC 4701 standard with DHCID record
    - Requires DHCP server to perform DDNS, not clients

#### Allowed Devices
Only a limited set of devices should be allowed to send DDNS updates. The less, the better!
- DHCP servers (and DHCP Failover peers)
- Active Directory domain controllers
- Cluster servers
- Trusted devices or server subnets

#### Warning
- Do not allow both DHCP client and DHCP server to update DNS, it creates a race condition
    - At best they overwrite each other's data
    - At worst the record is lost and not created. Especially if RFC 4701 is used, DHCID records are lost
- Choose either client or server, not both

## Troubleshooting Tips
### DHCP Troubleshooting Tips

### DNS Troubleshooting Tips
### DNS Network Considerations
### DNS Split Authority