## Security Overview
Computer Security is the protection of items of value (called assets) of a computer or computer system. There are many types of assets, involving hardware, software, data, process, staff, or a combination thereof.

Computer Security:
1. Integrates into the Assets Life Cycle: Security begins with acquiring assets. It is important that there is a chain of custody for the physical assets, up to and including when the asset is powered on.
2. Integrates into the Software Development Life Cycle: Software development life cycle is a process that includes Design, Development, Testing and Deployment. It is critical that security is considered during the design phase of a project.
3. Establishing Orgizational Policies and Procedures: This reduces the risk of accidental or intentional harm.
4. Defining Roles and Responsibilities: It is important to establish a hierarchical structure for incident response. There needs to be a defined leader or plan for assigning a leader due to the strenuous nature of the job.

### Basic Security Principles
While there are many ways of looking at system security, most fall into one of these principles:
![Security Principles](/images/cks/security_principles.png)

#### Assessment
Assessment is the operation of determining the value of assets and the cost of implementing security to protect those assets. The focus is generally on the most valuable assets. Because of limited resources, 100% security is impossible to achieve. Priorities will be assigned to specific assets based on their value.
Knowing the value of each asset and the cost of securing each item is critical in focusing the security resources.
Residual risks are those that are unsecured by controls post-deployment. These are generally not a prority or cost prohibitive items that are left unaddressed in the security framework. You should document them. Examples:
- Single Sign-On systems can introduce new security concerns.
- Additional security is itself a risk.
- Centralization of services introduces greater risk of downtime through single points of failure and attack.

#### Prevention
- Prevention of known risks is the easiest and likely the most cost effective principle to incorporate into a system.
- Prevention is the implementation of security measures, called **controls**, to protect assets identified during the assessment stage. Controls refer to software and strategies deployed to protect the assets. There are three types of controls:
1. **Technical controls** involve software and hardware to protect assets.
2. **Procedural controls** involve processes and policies designed to prevent loss and damage.
3. **Physical controls** involve facilities, staff, key cards, locks, and other measures.

For each asset, there is some aspect that needs to be protected. For example, for services: Does it need to be available? For data: Does it need to confidential? For new software: Can the integrity of the code and executables be assured?

There are an unlimited number of threats out in the world; they can be accidental, malicious, and directed. It is impossible to prevent them all, which is why the assessment stage is important.

#### Detection
Detection involves monitoring through the use of various technologies, such as remote logging, system statistics, and performance metrics. Detection can be the most expensive and can be diffiult to execute effectively.

Intrusion Detection and Prevention Systems (IDPS) are used to identify possible incidents, create a consistent audit trail, and report attempted intrusions.

Current incident detection methods include signature-based, statistical anomaly-based, and stateful protocol analysis.

Stateful protocol analysis includes system monitoring. Statistical anomaly based detection involves creating a baseline and monitoring for anomalies. Tools like Prometheus are helpful for production-wide metrics and a time-series database to view historical information. The use of production-wide logging is also essential to understand typical usage, as well as unusual activity.

#### Reaction
Reaction is one of the least considered principles, and catastrophes often result from poorly planned reactions to vulnerabilities and risk. How an incident is addressed may have long term effects and must be considered carefully.

While detection of system risks is critically important, how can organization reacts to these vulnerabilities can determine the survival of the system and, in some cases, the organization itself. Reaction can range from adding firewall rules, adding scanners, re-implementing the systems, or shutting down certain components. An inappropriate response can be disastrous in its technical effectiveness or simply in its perceived effectiveness. If users and consumers are not confident in an organization's reaction to a threat, they many move to other competitors.

Part of the reaction is ensuring business contiuity. Business continuity requires knowing which components are most important for the business. This topic is also addressed during the assessment phase of any security framework.

It is important to maintain a focus on problem solving. Work on solutions rather than specific blame. Blame is an inproductive activity. Unexpected failures will occur no matter how much planning and implementation is affected. Root cause analysis is productive for identifying technical issues.

### Classes of Attackers
#### White Hat
A white hat hacker breaks security for non-malicious reasons, perhaps to test their own security system or while working for a security company which makes security software. The term **white hat** is Internet slang refers to an ethical hacker. This classification also includes individuals who perform penetration tests and vulnerability assessments within a contractual agreement.

#### Black Hat
A black hat hacker is a hacker who violates computer security to be malicious or for personal gain. Black hat hackers form the stereotypical, illegal hacking groups often portrayed in popular culture. Black hat hackers break into secure networks to destroy data or make the network unusable for those who are authorized to use the network.

#### Script Kiddie
A script kiddie (also known as a skid or skiddie) is a non-expert who breaks into computer systems by using prepackaged automated tools written by others, usually with little understanding of the underlying concept.

#### Hacktivist
A hacktivist is a hacker who utilizes technology to announce a social, ideological, religious, or political message. In general, most hactivist involves website defacement or denial-of-service attacks.

#### Nation State
Nation state refers to intelligence agencies and cyber warfare operatives of nation states.

#### Organized Crime
Organized crime refers to criminal activities carried out for profit.

#### Bots
Bots are automated software tools that are available for use by any type of attacker.

### Attack Sources
An attack can be perpetrated by an insider or from outside the organization. An inside attack is an attack initiated by an entity inside the security perimeter (an insider), i.e., an entity that is authorized to access system resources but uses them in a way not approved by those who granted the authorization.

An outside attack is initiated from outside the perimeter, by an unauthorized or illegitimate user of the system (an outsider). On the internet, potential outside attackers range from amateur pranksters to organized criminals, international terrorists, and hostile governments.

A resource (both physical or logical), called an asset, can have one or more vulnerabilities that can be exploited by a threat agent in a threat action. The result can potentially compromise the Confidentiality, Integrity or Availability properties of resources (potentially different from the vulberable one) of the organization and other involved parties (customers, suppliers).

An attacker need not remove data for there to be a problem. Reading of material, such a secret formula, or encryption and extortion for the key could be done without removal of any data.

### Types of Attacks
#### Active Attacks
An attack is referred to as active when it attempts to alter system resources or affect their operation so it compromises Integrity or Availability.

Denial of service attacks are generally done by flooding the service or network with more requests than can be serviced and results in the service becoming unreachable. This sometimes happens due to a client misconfiguration.

Spoofing attacks take place when a valid or authorized system is impersonated via IP address manipulation. The service thinks it is communicating with an authorized system when it is really talking to an imposter. ARP, DNS, IP Address, and MAC are susceptible to spoofing.

Port scanning can be done with the nmap utility and involves sending SYN packets to a range of ports on the target systems. The replies, or lack of replies, from the target provide a significant amount of information about the possible services running on the target.

Idle scans are variations on port scans that use a third system, referred to as zombie, to gain information about a target system. There is quite a variety of network attacks that are still widely used that take advantage of various network protocols required in most infrastructure. ARP storms, session hijacking, packet injection are all active attack techniques.

#### Passive Attacks
A passive attack attempts to learn or make use of information from the system, but does not affect system resources; it compromises Confidentiality.

Wiretapping is generally done with **tcpdump** or **wireshark** to listen to traffic on the network. This is done by placing network interfaces into promiscuous mode in which all packets, all ones and zeros really, are passed to the tcpdump application to be decoded.

During normal operations, network interfaces throw away packets sent to them by the network devices when the destinations do not match those configured on the host. Pretty much all communications protocols and mechanisms are susceptible to wiretapping. This includes Ethernet, WiFi, USB, and cellular networks.

Remember that most buildings do not fully contain signals or sound. As a result, these can be detected from outside, in a process sometimes called wardriving. Secure locations will play music, tempest walls, and vibrate windows to prevent the glass being used for wiretapping. Both this infrastructure, as well as regular searching for unauthorized signals is part of comprehensive security.

### The 4Cs of Security
![4Cs of Security](/images/cks/4cs_of_security.png)
As each layer greatly affects the layers within, one cannot protect everything from the Code layer if there are vulnerabilities at the Cluster layer, as seen in the graphic below.

There are many cloud providers, each with their own security best practices and settings. While most are locked down, it may be a good idea to research the settings rather than trust. Should you host your own equipment, keeping them secure is a full-time job, and beyond the scope of this course.

The many components of a cluster each have their own possible security concerns, which will be discussed later in the course.

Container security relies on trusted code. This layer is a combination of container vulnerability scanning, image signing to ensure nothing has been modified, and also preventing the leveraging of elevated privileges past the least privileges required.

#### Other Exploits
Each of the components of the cloud could become a target for compromise, otherwise known as an attack surface. Some may not be obvious, such as compromising a door card reader and taking over other IT systems.

One way to ensure data security is to remove power and lock the whole system in a vault under guard. However, while secure, the system and the data are not easily accessible. Every environment will need to balance access and security. This evaluation should be a planned and ongoing activity.

With Kubernetes, the component which stores the cluster state, the authentication and authorization settings, and the one component that cannot be easily rebuilt is the etcd database. Extra care should be taken to back up and protect this component, as its compromise would provide the most access and control.

The network remains a primary concern, as most environments want their end user to access resources via a network. Care should be taken to only open required ports, protect API endpoints the end user does not require, and also have a plan to re-evaluate on a regular basis.

On Kubernetes worker nodes, which tend to be exposed in some manner to the outside world, the kubelet and kube-proxy pods have access to the control plane. Special care should be taken to ensure these are not compromised. Each of the other projects you install will have similar agents, each requiring planned and ongoing evaluation.

#### Hardware
Projects like Platform AbstRaction for SECurity (PARSEC) are trying to provide a common API to security services abstracted from the particular platform.
"Parsec aims to define a universal software standard for interacting with secure object storage and cryptography services, creating a common way to interface with functions that would traditionally have been accessed by more specialized APIs"

The project aims to allow applications to utilize primitives in a platform-agnostic way while also supporting a range of applications on the same system.

### NIST CyberSecurity Framework
There are quite a few publications to read in order to keep up with ongoing security threats. The Federal Information Processing Standard (FIPS) is a good place to start. At first, the thousands of pages of documentation may be overwhelming. It can take quite a while to read and digest all of the information. The CyberSecurity Framework (CSF) organizes security into five different activities (Identify, Protect, Detect, Respond, and Recovery), and provides information on finding vendors to assist with each. The first activity is Identify, for example. This is a grouping of organizational structures necessary to manage cybersecurity risk. They further divide the activity into six categories:
- Asset Management
- Business Environment
- Governance
- Risk Assessment
- Risk Management Stategy
- Supply Chain Risk Management

By breaking down the massive scale of cybersecurity into activities and categories, it may allow you to begin with the information most important to your current role, and expanding knowledge as time and effort allows.

### High Value Asset Protection
With a seemingly endless number of attack surfaces, it would be an impossible task to detect all phases of an attack from any possible actor, so time and energy should be spent starting with the most important High Value Asset (HVA), and supporting systems and applications. Then follow with the next layer of dependency. The more dynamic your environment, the more often a re-assessment should be performed.
As well, there should be pre-planning on how to return systems to operation and a healthy state should a compromise take place. Just as with assessing the HVAs, you should also have a plan to re-assess your return to typical business plan.
Another government agency, CyberSecurity and Infrastructure Security Agency (CISA) works to help governments and critical infrastructure organizations to minimize the risk of a cyber attack. They provide scanning and penetration testing to encourage the best cyber hygiene for federal, state, local, tribal, and territorial governments.

### National Checklist Program
The National Vulnerability Database (NVD) is a service provided by the National Institute of Standards and Technology (NIST), a US government physical sciences laboratory which also hosts the Computer Security Resource Center (CSRC), where Federal Information Processing Standards (FIPS) and (Special Publications (SP) documents can be found, among others.
In addition to the many documents available, you can search for known issues and checklists in their database, such as checklists for compliance, vulnerability, specialized issues, etc.

### CIS Benchmarks
The Center for Internet Security, Inc (CIS) is a non-profit organization working to share cybersecurity best practices, information, and tools. Some of the information is provided free of charge, other tools require a membership.
A popular tool which requires a membership is the CIS-CAT Pro, which can be run on a system to compare and report back conformance to best practices. The data is ordered in a numberical scale, and the dynamically-created web page offers information on how to improve conformance.
The organization also provides benchmarks which can be used to evaluate your systems and software on your own. These freely available tools can give information and perhaps a unique perspective on various security considerations.

#### kube-bench
If you are unable to download the full CIS tools, you can use the kube-bench tool from Aqua Security. While they make an effort to test for the same issues as from CIS, there may not be a 1-to-1 mapping, and not all tests may be included. Still, it would be better than not knowing of any vulnerabilities at all.
If you are using a managed cluster, like GKE, then the cloud provider runs the master node. Be aware they probably won't allow the testing to run.

### Improve Security Team Culture
Some organizations do not foster a healthy culture in their security organization. Perhaps for many reasons, some think they should default to denying a request. As a result, the rest of the organization avoids interacting with security because it will only "get in the way" of doing their job. Instead, a healthy organization will train to first respond something like "Yes, but here's how we make it more secure". This subtle change in approach starts the process of turning the security team into internal consultants, which the rest of the organization looks forward to working with. The more skills and knowledge your security team has, the better they can provide internal consulting and the more their participation will be valued.
If the security team is viewed as a warehouse of unwanted staff who always say no, the entire organization will suffer. If they become the best of the best, the organization will continually be considering security and fully integrating a security mindset.

### Limit Access
A central concept in security is limiting external access to the production environment. There is always a balance between allowing end users the appropriate amount of access without allowing more than the least access possible. This is made up of several layers, with the complexity and number of layers tied to the sensitivity of the information being protected.
Protecting network traffic is not just an edge decision. Every connection, both between nodes, as well as intra-node communication, should have a series of firewalls working in conjunction with each other.
A dynamic CI/CD environment may not be as protected due to the necessary dynamic nature of constant change. As a result, it is essential to insert scanning and verification tools as part of the pipeline, as well as ongoing reviews and assessment to ensure problems are properly caught and fixed.
There should also be a collection of non-container specific security tools (e.g., SELinux, Kerberos, SAML, etc) to manage access at each layer, from hardware up to the application.

### Tracking Known Issues
1. View [https://nvd.nist.gov/vuln/search](https://nvd.nist.gov/vuln/search) and type in `Kubernetes` as the keyword search.
2. Select a record which indicates a `CRITICAL` vulnerability, and read through current description and the product that it effects. Continue to read through other references, weakness enumeration, and known software configurations.
3. Determine who in your organization will be responsible for keeping track of CVE updates. Would it be one group, or a different person in working with individual project software.
4. Subscribe to RSS feeds or create integration with Slack to keep up with updates - [https://us-cert.cisa.gov](https://us-cert.cisa.gov)

### CIS-CAT Lite
CIS Offers free/paid tools to help you evaluate against your systems. CIS-CAT Lite a free tool. This section walks through its installation
1. Download the tool from [CIS website](https://www.cisecurity.org/)
2. Fill out the information and wait till you receive a download email
3. Copy the link and use wget to download the tool on your system
4. Rename the file using `mv` command, example below
```
mv 246234853\?h\=sUYmaWqrfZ1WJdNA13ORjsWCxHV1Ud6WohvYUFwYw7w CIS-CAT.zip
```
5. Download the following packages to run the tool, assuming ubuntu
```
sudo apt-get update
sudo apt-get install unzip
unzip CIS-CAT.zip
cd Assessor-CLI
sudo apt-get install openjdk-11-jdk -y
export JAVA_PATH=/usr/lib/jvm/java-11-openjdk-amd64/bin/
```
6. Run the `Assessor-CLI.sh` script without any options. You should see some warnings and a text graphic, followed by help information. Among that information note the levels of verbosity as well as the-ioption to run an interactiveassessment. Use the **sudo** command as the tool requires root ability.
```
sudo bash Assessor-CLI.sh
```
7. Run the program again, this time pass the -i option. When the Select Content prompt appears enter the number 5, to assess Ubuntu.
```
sudo bash Assessor-CLI.sh -i
```
```
Verifying application

Attempting to load the default sessions.properties, bundled with the application.
Started Assessment 1/1

Loading Benchmarks/Data-Stream Collections

Available Benchmarks/Data-Stream Collections:
 1. CIS Controls Assessment Module - Implementation Group 1 for Windows 10 v1.0.3
 2. CIS Controls Assessment Module - Implementation Group 1 for Windows Server v1.0.0
 3. CIS Google Chrome Benchmark v2.1.0
 4. CIS Microsoft Windows 10 Enterprise Release 21H1 Benchmark v1.11.0
 5. CIS Ubuntu Linux 18.04 LTS Benchmark v2.1.0
 > Select Content # (max 5): 5
 ```

 8. You should then see options about what level of testing you want to do. Chose option 1. There will be a lot of output following. Take a moment to scan through the hundreds of tests. Some will pass, some will fail. At the end of the output you should see total assessment time and a location for the HTML report.
 ```
 Selected 'CIS Ubuntu Linux 18.04 LTS Benchmark'

Dec 26, 2021 10:47:56 AM com.sun.org.slf4j.internal.Logger warn
WARNING: The input bytes to the digest operation are null. This may be due to a problem with the Reference URI or its Transforms.
Assessment File CIS_Ubuntu_Linux_18.04_LTS_Benchmark_v2.1.0-xccdf.xml has a valid Signature.
Profiles:
1. Level 1 - Server
2. Level 2 - Server
3. Level 1 - Workstation
4. Level 2 - Workstation
 > Select Profile # (max 4): 1

Selected Profile 'Level 1 - Server'

Obtaining session connection --> Local
Connection established.
Selected Checklist 'CIS Ubuntu Linux 18.04 LTS Benchmark'
Selected Profile 'Level 1 - Server'
Starting Assessment
```
9. Assuming this is running on a publicly assessible EC2, run the following command to run http server from the folder /Assessor-CLI
```
python3 -m http.server 8000
```
10. Access the report, else use SCP/SFTP to transfer the file to your local machine for viewing.