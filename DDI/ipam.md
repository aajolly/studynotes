# IPAM
## Overview
- IP Address Management (IPAM) starts with a simple question:
    - Who has which IP address?
- Network and security administrators often face these challenges:
    - How full iso ur network? Is it time to expand?
    - How many wireless users do we have? Is it time to upgrade?
    - Who has this MAC address? Is it a printer or a desktop? Where is it?

![ipam-history](/images/ddi/ipam-history.png)

### Spreasheets for IP Address Management?
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

### Role of search, report & alerts in IPAM System
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