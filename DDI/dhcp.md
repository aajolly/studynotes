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

### DHCP Policy
### Deployment Recommendations

## DHCP Timers and Lease States
### DHCP Lease
### DHCP Timers and Lease States
### DHCPv6 Lease Lifetime and Timers
### Address Binding States

## DHCP Options
### DHCP Options
### Undefined Options
### Vendor Specific Options
### DHCPv6 Options

## Dynamic DNS
### DDNS Update Process
### DHCP Option 81
### DDNS Considerations

## Troubleshooting Tips
### DHCP Troubleshooting Tips
### DNS Troubleshooting Tips
### DNS Network Considerations
### DNS Split Authority