# Malware Types
Attackers use a variety of techniques and attack types to achieve their objectives. Malware and exploits are integral to the modern cyberattack strategy.

## Overview
Malware (short for “malicious software”) is a file or code that typically takes control of, collects information from, or damages an infected endpoint.
- **Malware** usually has one or more of the following objectives: to provide remote control for an attacker to use an infected machine, to send spam from the infected machine to unsuspecting targets, to investigate the infected user’s local network, and to steal sensitive data.
- **Advanced or modern malware** generally refers to new or unknown malware. These types of malware are highly sophisticated and often have specialized targets. Advanced malware typically can bypass traditional defenses.

Malware is varied in type and capabilities.

- A **logic bomb** is malware that is triggered by a specified condition, such as a given date or a particular user account being disabled.

- **Spyware and adware** are types of malware that collect information, such as internet surfing behavior, login credentials, and financial account information, on an infected endpoint. Spyware often changes browser and other software settings and slows computer and internet speeds on an infected endpoint. Adware is spyware that displays annoying advertisements on an infected endpoint, often as pop-up banners.

- A **rootkit** is malware that providers privileged (root-level) access to a computer. Rootkits are installed in the BIOS, which mans operating system-level security tools cannot detect them.

- A **bootkit** is malware that is a kernel-mode variant of a rootkit, commonly used to attack computers that are protected by full-disk encryption.

- A **backdoor** is malware that allows an attacker to bypass authentication to gain access to a compromised system.

- **Anti-AV** is malware that disables legitimately installed antivirus software on the compromised endpoint, thereby preventing automatic detection and removal of other malware.

- **Ransomware** is malware that locks a computer or device (Locker ransomware) or encrypts data (Crypto ransomware) on an infected endpoint with an encryption key that only the attacker knows, thereby making the data unusable until the victim pays a ransom (usually with cryptocurrency, such as Bitcoin). Reveton and LockeR are two examples of Locker ransomware. Locky, TeslaCrypt/EccKrypt, Cryptolocker, and Cryptowall are examples of Crypto ransomware.

- A **Trojan horse** is malware that is disguised as a harmless program but actually gives an attacker full control and elevated privileges of an endpoint when installed. Unlike other types of malware, Trojan horses are typically not self-replicating.

- A **virus** is malware that is self-replicating but must first infect a host program and be executed by a user or process.

- A **worm** is malware that typically targets a computer network by replicating itself to spread rapidly. Unlike viruses, worms do not need to infect other programs and do not need to be executed by a user or process.

### Advanced or Modern Malware
Advanced or modern malware leverages networks to gain power and resilience. Modern malware can be updated—just like any other software application—so that an attacker can change course and dig deeper into the network or make changes and enact countermeasures.

This is a fundamental shift compared to earlier types of malware, which were generally independent agents that simply infected and replicated themselves.

Below are some characteristics of the more advanced malware. 
- Advanced malware often uses common **obfuscation** techniques to hide certain binary strings that are characteristically used in malware and therefore easily detected by anti-malware signatures. Advanced malware might also use these techniques to hide an entire malware program.
- Some advanced malware has entire sections of code that serve no purpose other than to change the signature of the malware, thus producing an infinite number of unique signature hashes. Techniques such as **polymorphism** and metamorphism are used to avoid detection by traditional signature-based anti-malware tools and software. For example, a change of just a single character or bit of the file or source code completely changes the hash signature of the malware.
- Advanced malware takes full advantage of the resiliency built into the internet itself. Advanced malware can have multiple control servers **distributed** all over the world with multiple fallback options. Advanced malware can also leverage other infected endpoints as communication channels, thus providing a near-infinite number of communication paths to adapt to changing conditions or update code as needed.
- Updates from C2 servers can also completely change the functionality of advanced malware. This **multifunctional** capability enables an attacker to use endpoints strategically to accomplish specific tasks, such as stealing credit card numbers, sending spam containing other malware payloads (such as spyware), or installing ransomware for the purpose of extortion.

## Ransomware Types
### Overview
Although cryptographic ransomware is the most common and successful type of ransomware, it is not the only one. It’s important to remember that ransomware is not a single family of malware but is a criminal business model in which malware is used to hold something of value for ransom.

While holding something of value for ransom is not a new concept, ransomware has become a multibillion-dollar criminal business targeting both individuals and corporations. Due to its low barriers to entry and effectiveness in generating revenue, it has quickly displaced other cybercrime business models and has become the largest threat facing organizations today. It is also important to note that although threat actors generally do decrypt your data after the ransom is paid (the ransomware business model depends on a reasonable expectation that paying a ransom will restore access to your data), there are no guarantees that this will be the case. Additionally, many threat actors are now exfiltrating a copy of their victims’ data – particularly PII and credit card numbers – before encrypting it, then selling the data on the dark web after the ransom is paid.

Though the malware deployed in the current generation of cryptographic ransomware attacks is not especially sophisticated, it has proven very effective at not only generating revenue for the criminal operators but also preventing impacted organizations from continuing their normal operations. New headlines each week demonstrate that organizations large and small are vulnerable to these threats, enticing new attackers to jump onto the bandwagon and begin launching their own ransomware campaigns.

For a ransomware attack to be successful, attackers must execute the following five steps.

If the attacker fails in any of these steps, the scheme will be unsuccessful. Although the concept of ransomware has existed for decades, the technology and techniques, such as reliable encrypting and decrypting, required to complete all five of these steps on a wide scale were not available until just a few years ago. Click the arrows for more information about the five steps.
1. **Compromise and Control a System or Device**: Ransomware attacks typically begin by using social engineering to trick users into opening an attachment or viewing a malicious link in their web browser. This allows attackers to install malware onto a system and take control. However, another increasingly common tactic is for attackers to gain access to the network, perform reconnaissance on the network to identify potential targets and establish Command and Control (C2), install other malware and create backdoor accounts for persistence, and potentially exfiltrate data.
2. **Prevent Access to the System**: Attackers will either identify and encrypt certain file types or deny access to the entire system.
3. **Notify Victim**: Though seemingly obvious, attackers and victims often speak different languages and have varying levels of technical capabilities. Attackers must alert the victim about the compromise, state the demanded ransom amount, and explain the steps for regaining access.
4. **Accept Ransom Payment**: To receive payment while evading law enforcement, attackers utilize cryptocurrencies such as Bitcoin for the transaction.
5. **Return Full Access**: Attackers must return access to the device(s). Failure to restore the compromised systems destroys the effectiveness of the scheme as no one would be willing to pay a ransom if they didn’t believe their valuables would be returned.

## Vulnerabilities and Exploits
Vulnerabilities and exploits can be leveraged to force software to act in ways it’s not intended to, such as gleaning information about the current security defenses in place.

### Vulnerability
Vulnerabilities are routinely discovered in software at an alarming rate. Vulnerabilities may exist in software when the software is initially developed and released, or vulnerabilities may be inadvertently created, or even reintroduced, when subsequent version updates or security patches are installed.  

### Exploit
An exploit is a type of malware that takes advantage of a vulnerability in an installed endpoint or server software such as a web browser, Adobe Flash, Java, or Microsoft Office. An attacker crafts an exploit that targets a software vulnerability, causing the software to perform functions or execute code on behalf of the attacker.

#### Vulnerabilities Patching
Security patches are developed by software vendors as quickly as possible after a vulnerability has been discovered in their software.  
1. **Discovery**: An attacker may learn of a vulnerability and begin exploiting it before the software vendor is aware of the vulnerability or has an opportunity to develop a patch.
2. **Development of Patch**: The delay between the discovery of a vulnerability and development and release of a patch is known as a zero-day threat (or exploit).
3. **Test and Deploy Patch**: It may be months or years before a vulnerability is announced publicly. After a security patch becomes available, time inevitably is required for organizations to properly test and deploy the patch on all affected systems. During this time, a system running the vulnerable software is at risk of being exploited by an attacker.

#### How exploits are executed
Exploits can be embedded in seemingly innocuous data files (such as Microsoft Word documents, PDF files, and webpages), or they can target vulnerable network services. Exploits are particularly dangerous because they are often packaged in legitimate files that do not trigger anti-malware (or antivirus) software and are therefore not easily detected.
1. **Creation** of an exploit data file is a two-step process. The first step is to embed a small piece of malicious code within the data file. However, the attacker still must trick the application into running the malicious code. Thus, the second part of the exploit typically involves memory corruption techniques that allow the attacker’s code to be inserted into the execution flow of the vulnerable software.
2. After the exploit data file is created, a legitimate application, such as a document viewer or web browser, will perform actions on behalf of the attacker, such as establishing communication and providing the ability to upload additional malware to the target endpoint. Because the application being exploited is a legitimate application, traditional signature-based antivirus and whitelisting software have virtually no defense against these attacks.
3. Although there are thousands of exploits, they all rely on a small set of core techniques. Some attacks may involve more steps, and some may involve fewer, but typically three to five core techniques must be used to exploit an application. Regardless of the attack or its complexity, for the attack to be successful the attacker must execute a series of these core exploit techniques in sequence.
4. Heap spray is a technique used to facilitate arbitrary code execution by injecting a certain sequence of bytes into the memory of a target process.

#### Timeline of Eliminating a Vulnerability
Vulnerabilities can be exploited from the time software is deployed until it is patched.
1. **Software Deployed**: For local systems, the only way to eliminate vulnerabilities is to effectively patch systems and software.
2. **Vulnerability Discovered**: Security patches are developed by software vendors as quickly as possible after a vulnerability has been discovered in their software.
3. **Exploits Begin**: The process of discovery and patching will continue. According to research by Palo Alto Networks, 78 percent of exploits take advantage of vulnerabilities that are less than two years old, which implies that developing and applying patches is a lengthy process.
4. **Public Announcement of Vulnerability**: An attacker may learn of a vulnerability and begin exploiting it before the software vendor is aware of the vulnerability or has an opportunity to develop a patch.
5. **Patch Released**: This delay between the discovery of a vulnerability and development and release of a patch is known as a zero-day threat (or exploit).
6. **Patch Deployed**: Months or years could pass by before a vulnerability is announced publicly. After a security patch becomes available, time inevitably is required for organizations to properly test and deploy the patch on all affected systems.
7. **Protected by Vendor Patch**: During this time, a system running the vulnerable software is at risk of being exploited by an attacker.

# Cyberattack Techniques
## Overview
Attackers use a variety of techniques and attack types to achieve their objectives. Spamming and phishing are commonly employed techniques to deliver malware and exploits to an endpoint via an email executable or a web link to a malicious website. Once an endpoint is compromised, an attacker typically installs back doors, remote access Trojans (RATs), and other malware to ensure persistence. This lesson describes spamming and phishing techniques, how bots and botnets function, and the different types of botnets.
### Business email compromise (BEC) 
It is one of the most prevalent types of cyberattacks that organizations face today. The FBI Internet Crime Complaint Center (IC3) estimates that "in aggregate" BEC attacks cost organizations three times more than any other cybercrime and BEC incidents represented nearly a third of the incidents investigated by Palo Alto Networks Unit 42 Incident Response Team. According to the Verizon Data Breach Investigations Report (DBIR), BEC is the second most common form of social engineering today.
-- Spam and phishing emails are the most common delivery methods for malware. The volume of spam email as a percentage of total global email traffic fluctuates widely from month to month – typically 45 to 75 percent. Although most end users today are readily able to identify spam emails and are savvier about not clicking links, opening attachments, or replying to spam emails, spam remains a popular and effective infection vector for the spread of malware. Phishing attacks, in contrast to spam, are becoming more sophisticated and difficult to identify. 
![definitions](/images/security/bec.png)

### Phishing Attacks
We often think of spamming and phishing as the same thing, but they are actually separate processes, and they each require their own mitigations and defenses. Phishing attacks, in contrast to spam, are becoming more sophisticated and difficult to identify.
- **Spear phishing** is a targeted phishing campaign that appears more credible to its victims by gathering specific information about the target, giving it a higher probability of success. A spear phishing email may spoof an organization (such as a financial institution) or individual that the recipient actually knows and does business with. It may also contain very specific information (such as the recipient’s first name, rather than just an email address).  
Spear phishing, and phishing attacks in general, are not always conducted via email. A link is all that is required, such as a link on Facebook or a message board or a shortened URL on Twitter. These methods are particularly effective in spear phishing attacks because they allow the attacker to gather a great deal of information about the targets and then lure them through dangerous links into a place where the users feel comfortable.
- **Whaling** is a type of spear phishing attack that is specifically directed at senior executives or other high-profile targets within an organization. A whaling email typically purports to be a legal subpoena, customer complaint, or other serious matter.
- **Watering hole** attacks compromise websites that are likely to be visited by a targeted victim-for example, an insurance company website that may be frequently visited by healthcare providers. The compromised website will typically infect unsuspecting visitors with malware (known as a “drive-by download”).  
- A **pharming** attack redirects a legitimate website’s traffic to a fake site, typically by modifying an endpoint’s local hosts file or by compromising a DNS server (DNS poisoning).

### Bots and botnets
Bots and botnets are notoriously difficult for organizations to detect and defend against using traditional anti-malware solutions.
- **Bots** (or zombies) are individual endpoints that are infected with advanced malware that enables an attacker to take control of the compromised endpoint.
- A **Botnet** is a network of bots (often tens of thousands or more) working together under the control of attackers using numerous servers.
- In a botnet, advanced malware works together toward a common objective, with each bot growing the power and destructiveness of the overall botnet. The botnet can evolve to pursue new goals or adapt as different security countermeasures are deployed. Communication between the individual bots and the larger botnet through C2 servers provides resiliency in the botnet.
- Given their flexibility and ability to evade defenses, botnets present a significant threat to organizations. The ultimate impact of a botnet is largely left up to the attacker, from sending spam one day to stealing credit card data the next. Because many cyberattacks go undetected for months or even years, botnets can cause a great deal of damage.

#### Disabling a botnet
Botnets themselves are dubious sources of income for cybercriminals. Botnets are created by cybercriminals to harvest computing resources (bots). Control of botnets (through C2 servers) can then be sold or rented out to other cybercriminals.
**Challenges to disabling a botnet**
The key to “taking down” or “decapitating” a botnet is to separate the bots (infected endpoints) from their brains (C2 servers). If the bots cannot get to their servers, they cannot get new instructions, upload stolen data, or do anything that makes botnets so unique and dangerous. Although this approach may seem straightforward, disabling a botnet presents many challenges.
1. **Extensive resources** are typically required to map the distributed C2 infrastructure of a botnet. Mapping a botnet's infrastructure almost always requires an enormous amount of investigation, expertise, and coordination between numerous industry, security, and law enforcement organizations worldwide.
2. Disabling C2 servers often requires both physically seizing the servers and taking ownership of the domain and IP address range associated with the servers. Very close coordination between technical teams, legal teams, and law enforcement is essential to disabling the C2 infrastructure of a botnet. Many botnets have C2 servers all over the world and will specifically function in countries that have little or no law enforcement for internet crimes.
3. A botnet almost never relies on a single C2 server but rather uses multiple C2 servers for redundancy purposes. Each server also is typically insulated by a variety of intermediaries to cloak the true location of the server. These intermediaries include P2P networks, blogs, social networking sites, and even communications that proxy through other infected bots. These evasion techniques make even finding C2 servers a considerable challenge.
4. Most botnets are designed to withstand the loss of a C2 server, meaning that the entire botnet C2 infrastructure must be disabled almost simultaneously. If any C2 server is accessible or any of the fallback options survive, the bots will be able to get updates and rapidly populate a completely new set of C2 servers, and the botnet will quickly recover. Thus, even a single C2 server remaining functional for even a small amount of time can give an attacker the window needed to update the bots and recover the entire botnet.
5. Botnet C2 servers are used to control infected endpoints (bots) and to exfiltrate personal and valuable data from bots. Botnets can be easily scaled up to send massive volumes of spam, spread ransomware, launch distributed denial-of-service (DDoS) attacks, commit clickfraud campaigns, or mine cryptocurrency (such as Bitcoin).

#### Actions for Disabling a Botnet
The following are actions for disabling a botnet. 
> **Note**: Effectively deterring a botnet infection may be an ongoing process.
1. **Disabling internet access** is a highly recommended first action, along with aggressively monitoring local network activity to identify the infected devices. The first response to discovery of infected devices is to remove them from the network, thus severing any connections to a C2 server and keeping the infection from spreading.
2. **Monitor Local Network Activity**: The next response is to ensure that current patches and updates are applied. If infected endpoints are still persistently attempting to connect to a C2 service or an attack target, then the endpoints should be imaged and cleansed.
3. **Remove Infected Devices and Botnet Software**: Effective deterrent of a botnet infection may be an ongoing process. Devices may return to a dormant state and appear to be clean of infection for prolonged periods of time, only to one day be “awakened” by a signal from a C2 service.
4. **Install Current Patches**: The Internet Service Provider (ISP) community has a commitment to securing internet backbones and core services known as the Shared Responsibility Model. Adhering to this model does not ensure that ISP providers can fully identify and disable C2 service clusters. Full termination of C2 architecture can be extremely difficult.

#### Spamming botnets
The largest botnets are often dedicated to sending spam. The premise is straightforward: The attacker attempts to infect as many endpoints as possible, and the endpoints can then be used to send out spam email messages without the end users’ knowledge.
- **Productivity**: The relative impact of this type of bot on an organization may seem low initially, but an infected endpoint sending spam could consume additional bandwidth and ultimately reduce the productivity of the users and even the network itself.
- **Reputation**: Perhaps more consequential is the fact that the organization’s email domain and IP addresses could easily become listed by various real-time blackhole lists (RBLs), causing legitimate emails to be labeled as spam and blocked by other organizations and damaging the reputation of the organization.

### Distributed Denial of Service Attack (DDoS)
A DDoS attack is a type of cyberattack in which extremely high volumes of network traffic such as packets, data, or transactions are sent to the target victim’s network to make their network and systems (such as an e-commerce website or other web application) unavailable or unusable.

A DDoS botnet uses bots as part of a DDoS attack, overwhelming a target server or network with traffic from a large number of bots. In such attacks, the bots themselves are not the target of the attack. Instead, the bots are used to flood some other remote target with traffic. The attacker leverages the massive scale of the botnet to generate traffic that overwhelms the network and server resources of the target.  
Unlike other types of cyberattacks, a DDoS attack does not typically employ a prolonged, stealthy approach. Instead, a DDoS attack more often takes the form of a highly visible bruteforce attack that is intended to rapidly cause damage to the victim’s network and systems infrastructure and to their business and reputation.

**Financial botnets**, such as ZeuS and SpyEye, are responsible for the direct theft of funds from all types of enterprises. These types of botnets are typically not as large as spamming or DDoS botnets, which grow as large as possible for a single attacker. Click the tabs for more information about where financial botnets are sold and their impact.

## Advanced Persistent Threats
Advanced persistent threats, or APTs, are a class of threats that are far more deliberate and potentially devastating than other types of cyberattacks. APTs are generally coordinated events that are associated with cybercriminal groups.
- Attackers use advanced malware and exploits. They typically also have the skills and resources necessary to develop additional cyberattack tools and techniques.
- An APT may take place over a period of several years. Attackers pursue specific objectives and move slowly and methodically to avoid detection.
- An APT is deliberate and focused, rather than opportunistic. APTs are designed to cause real damage.

## Wi-Fi Challenges
**Public Airwaves**: Wi-Fi is conducted over public airwaves. The 2.4GHz and 5GHz frequency ranges that are set aside for Wi-Fi communications are also shared with other technologies, such as Bluetooth. As a result, Wi-Fi is extremely vulnerable to congestion and collisions.
**Wi-Fi Network**: Additional problems exist because Wi-Fi device settings and configurations are well known, published openly, shared, and even broadcast. To begin securing a WLAN network, you should disable the Service Set Identifier Broadcast configuration. If the SSID is configured to broadcast, it is easier for an attacker to define simple attack targets and postures because the network is already discoverable.
**Mobile Device & Customer Apps**: Mobile devices themselves have significant vulnerabilities. Mobile device management is difficult to maintain when end users use "bring-your-own-device" features. For many users, patching and securing their mobile devices is an afterthought, and they often consider convenience and performance before security. End users often install apps that bring significant risk to both the device and the network, and they often disable security features that impact device performance.

## Wireless Security
Wi-Fi security begins—and ends—with authentication. An organization cannot protect its digital assets if it cannot control who has access to its wireless network.

### Effective Wireless Security
1. **Limit Access Through Authentication**: Complications with Wi-Fi and wireless network participation are defined by the devices themselves. Whenever possible, apply mobile device management to ensure devices are properly hardened and to limit the types of applications (particularly sharing and social networking services) that end users can install and use.
2. **Secure Content via Encryption**: Design of the client-to-access-point security association also needs careful attention. Combining advanced authentication methods, such as two-factor authentication, with strong encryption should ensure that only authorized users and devices are allowed to connect on the network.
3. **Participate in Known or Constrained Networks**: Although wired network boundaries use cabling and segmentation, wireless networks are conducted over open airwaves. A primary paradigm of wireless networking security is to limit network availability and discovery, which can be accomplished by not broadcasting the wireless network presence and availability, or SSID. Placement of wireless access points must be considered carefully, with the goal of limiting the range of a wireless network.

#### Security Protocols
- **Wired Equivalent Privacy (WEP)**: The WEP encryption standard is no longer secure enough for Wi-Fi networks. WPA2 and the emerging WPA3 standards provide strong encryption capabilities and manage secure authentication via the 802.1x standard.
As mobile device processors have advanced to handle 64-bit computing, AES as a scalable symmetric encryption algorithm solves the problems of managing secure, encrypted content on mobile devices.
- **WPA2**: WPA2-PSK supports 256-bit keys, which require 64 hexadecimal characters. Because requiring users to enter a 64-hexadecimal character key is impractical, WPA2 includes a function that generates a 256-bit key based on a much shorter passphrase created by the administrator of the Wi-Fi network and the SSID of the AP used as a salt (random data) for the one-way hash function.
- **WPA3**: WPA3 was published in 2018. Its security enhancements include more robust bruteforce attack protection, improved hotspot and guest access security, simpler integration with devices that have limited or no user interface (such as IoT devices), and a 192-bit security suite. Newer Wi-Fi routers and client devices will likely support both WPA2 and WPA3 to ensure backward compatibility in mixed environments.
According to the Wi-Fi Alliance, WPA3 features include improved security for IoT devices such as smart bulbs, wireless appliances, smart speakers, and other screen-free gadgets that make everyday tasks easier.

#### Evil Twin
Perhaps the easiest way for an attacker to find a victim to exploit is to set up a wireless access point that serves as a bridge to a real network. An attacker can inevitably bait a few victims with “free Wi-Fi access.”

Baiting a victim with free Wi-Fi access requires a potential victim to stumble on the access point and connect. The attacker can’t easily target a specific victim, because the attack depends on the victim initiating the connection. Attackers now try to use a specific name that mimics a real access point. Click the arrows for more information about how the Evil Twin attack is executed.
1. **Mimic a real access point**: A variation on this approach is to use a more specific name that mimics a real access point normally found at a particular location–the Evil Twin. For example, if a local airport provides Wi-Fi service and calls it “Airport Wi-Fi,” the attacker might create an access point with the same name using an access point that has two radios.
2. **Catch a greater number of users**: Average users cannot easily discern when they are connected to a real access point or a fake one, so this approach would catch a greater number of users than a method that tries to attract victims at random. Still, the user has to select the network, so a bit of chance is involved in trying to reach a particular target.
3. **Reach a large number of people; not targeted**: The main limitation of the Evil Twin attack is that the attacker can’t choose the victim. In a crowded location, the attacker will be able to get a large number of people connecting to the wireless network to unknowingly expose their account names and passwords. However, it’s not an effective approach if the goal is to target employees in a specific organization.

#### Jasager
To understand a more targeted approach than the Evil Twin attack, think about what happens when you bring your wireless device back to a location that you’ve previously visited. 
**Jasager Attack**: When you bring your laptop home, you don’t have to choose which access point to use, because your device remembers the details of wireless networks to which it has previously connected. The same goes for visiting the office or your favorite coffee shop. 

#### SSLstrip
After a user connects to a Wi-Fi network that’s been compromised–or to an attacker’s Wi-Fi network masquerading as a legitimate network–the attacker can control the content that the victim sees. The attacker simply intercepts the victim’s web traffic, redirects the victim’s browser to a web server that it controls, and serves up whatever content the attacker desires.
- **Emotet** is a Trojan, first identified in 2014, that has long been used in spam botnets and ransomware attacks. Recently, it was discovered that a new Emotet variant is using a Wi-Fi spreader module to scan Wi-Fi networks looking for vulnerable devices to infect. The Wi-Fi spreader module scans nearby Wi-Fi networks on an infected device and then attempts to connect to vulnerable Wi-Fi networks via a brute-force attack. After successfully connecting to a Wi-Fi network, Emotet then scans for non-hidden shares and attempts another brute-force attack to guess usernames and passwords on other devices connected to the network. It then installs its malware payload and establishes C2 communications on newly infected devices.
- **SSLstrip** strips SSL encryption from a “secure” session. When a user connected to a compromised Wi-Fi network attempts to initiate an SSL session, the modified access point intercepts the SSL request.
With SSLstrip, the modified access point displays a fake padlock in the victim’s web browser. Webpages can display a small icon called a favicon next to a website address in the browser’s address bar. SSLstrip replaces the favicon with a padlock that looks like SSL to an unsuspecting user.

## WiFi Attacks
There are different types of Wi-Fi attacks that hackers use to eavesdrop on wireless network connections to obtain credentials and spread malware.  
1. **Doppelganger** is an insider attack that targets WPA3-Personal protected Wi-Fi networks. The attacker spoofs the source MAC address of a device that is already connected to the Wi-Fi network and attempts to associate with the same wireless access point.
2. **Cookie Guzzler**: Muted Peer and Hasty Peer are variants of the cookie guzzler attack which exploit the Anti-Clogging Mechanism (ACM) of the Simultaneous Authentication of Equals (SAE) key exchange in WPA3-Personal.  

# Security Models
## Perimeter-Based Security Model
Perimeter-based network security models date back to the early mainframe era (circa late 1950s), when large mainframe computers were located in physically secure “machine rooms.” These rooms could be accessed by a limited number of remote job entry (RJE) terminals directly connected to the mainframe in physically secure areas.

**Relies on Physical Security**
Today’s data centers are the modern equivalent of machine rooms, but perimeter-based physical security is no longer sufficient.

- **Mainframe computers** predate the internet. In fact, mainframe computers predate ARPANET, which predates the internet. Today, an attacker uses the internet to remotely gain access, instead of physically breaching the data center perimeter.
- The primary value of the mainframe computer was its **processing power**. The relatively limited data that was produced was typically stored on near-line media, such as tape. Today, data is the target. Data is stored online in data centers and in the cloud, and it is a high-value target for any attacker.
- **Data centers** today are remotely accessed by millions of remote endpoint devices from anywhere and at any time. Unlike the RJEs of the mainframe era, modern endpoints (including mobile devices) are far more powerful than many of the early mainframe computers and are themselves targets.

**Assumes Trust on Internal Network**
The primary issue with a perimeter-based network security strategy, which deploys countermeasures at a handful of well-defined entrance and exit points to the network, is that the strategy relies on the assumption that everything on the internal network can be trusted.
- Remote employees, mobile users, and cloud computing solutions blur the distinction between “internal” and “external.”
- Wireless technologies, partner connections, and guest users introduce countless additional pathways into network branch offices, which may be located in untrusted countries or regions.
- Insiders, whether intentionally malicious or just careless, may present a very real security threat.
- Sophisticated cyberthreats could penetrate perimeter defenses and gain free access to the internal network.
- Malicious users can gain access to the internal network and sensitive resources by using the stolen credentials of trusted users.
- Internal networks are rarely homogeneous. They include pockets of users and resources with different levels of trust or sensitivity, and these pockets should ideally be separated (for example, research and development and financial systems versus print or file servers).

**Allows Unwanted Traffic**
A broken trust model is not the only issue with perimeter-centric approaches to network security. Another contributing factor is that traditional security devices and technologies (such as port-based firewalls) commonly used to build network perimeters let too much unwanted traffic through. 
Shortcomings of perimeter-based security:
1. **Application Control**: Cannot definitively distinguish good applications from bad ones (which leads to overly permissive access control settings)
2. **Encrypted Traffic**: Does not adequately account for encrypted application traffic
3. **Identify Users**: Do not accurately identify and control users (regardless of where they’re located or which devices they’re using)
4. **Protect against attacks**: Do not filter allowed traffic for known application-borne threats or unknown threats

**Conclusion**: Re-architecting defenses to create pervasive internal trust boundaries is, by itself, insufficient. Organizations must also ensure that the devices and technologies used to implement these boundaries provide the visibility, control, and threat inspection capabilities needed to securely enable essential business applications while still thwarting modern malware, targeted attacks, and the unauthorized exfiltration of sensitive data.

## Zero Trust Security Model
The Zero Trust security model addresses some of the limitations of perimeter-based network security strategies by removing the assumption of trust from the equation.

With a Zero Trust model, essential security capabilities are deployed in a way that provides policy enforcement and protection for all users, devices, applications, and data resources, as well as the communications traffic between them, regardless of location. 
- **No default trust**: With Zero Trust there is no default trust for any entity – including users, devices, applications, and packets – regardless of what it is and its location on or relative to the enterprise network.
- **Monitor and inspect**: The need to "always verify" requires ongoing monitoring and inspection of associated communication traffic for subversive activities (such as threats).
- **Compartmentalize**: Zero Trust models establish trust boundaries that effectively compartmentalize the various segments of the internal computing environment. The general idea is to move security functionality closer to the pockets of resources that require protection. In this way, security can always be enforced regardless of the point of origin of associated communications traffic.

**Benefits of the Zero Trust Model**
In a Zero Trust model, verification that authorized entities are always doing only what they’re allowed to do is not optional: It's mandatory.
- **Improved Effectiveness**: Clearly improved effectiveness in mitigating data loss with visibility and safe enablement of applications, plus detection and prevention of cyberthreats
- **Greater efficiency** for achieving and maintaining compliance with security and privacy mandates by using trust boundaries to segment sensitive applications, systems, and data
- **Improved ability** to securely enable transformative IT initiatives, such as user mobility, bring your own device (BYOD) and bring your own access (BYOA) policies, infrastructure virtualization, and cloud computing
- **Lower total cost of ownership** with a consolidated and fully integrated security operating platform, rather than a disparate array of siloed, purpose-built security point products

### Zero Trust Design Principles
The principle of least privilege in network security requires that only the permission or access rights necessary to perform an authorized task are granted.

**Core Zero Trust Principles**
Security profiles are defined based on an initial security audit performed according to Zero Trust inspection policies. Discovery is performed to determine which privileges are essential for a device or user to perform a specific function.
1. **Ensure Resource Access**: Ensure that all resources are accessed securely, regardless of location. This principle suggests the need for multiple trust boundaries and increased use of secure access for communication to or from resources, even when sessions are confined to the “internal” network. It also means ensuring that the only devices allowed access to the network have the correct status and settings, have an approved VPN client and proper passcodes, and are not running malware.
2. **Enforce Access Control**: Adopt a least privilege strategy and strictly enforce access control. The goal is to minimize allowed access to resources to reduce the pathways available for malware and attackers to gain unauthorized access.
3. **Inspect and Log All Traffic**: This principle reiterates the need to “always verify” while also reinforcing that adequate protection requires more than just strict enforcement of access control. Close and continuous attention must also be given to exactly what “allowed” applications are actually doing, and the only way to accomplish these goals is to inspect the content for threats.

### Zero Trust Architecture
The Zero Trust model identifies a protect surface made up of the network’s most critical and valuable data, assets, applications, and services (DAAS). Protect surfaces are unique to each organization. Because the protect surface contains only what’s most critical to an organization’s operations, the protect surface is orders of magnitude smaller than the attack surface–and always knowable.

- **Identify the Traffic**: With an understanding of the interdependencies among an organization's DAAS, infrastructure, services, and users, the security team should put controls in place as close to the protect surface as possible, creating a micro-perimeter around it. This micro-perimeter moves with the protect surface, wherever it goes.
- The **Zero Trust segmentation platform** (also called a network segmentation gateway by Forrester Research) is the component used to define internal trust boundaries. That is, the platform provides the majority of the security functionality needed to deliver on the Zero Trust operational objectives.

#### Conceptual Architecture
With the protect surface identified, security teams can identify how traffic moves across the organization in relation to the protect surface. Understanding who the users are, which applications they are using, and how they are connecting is the only way to determine and enforce policy that ensures secure access to data
- **Fundamental Assertions**
    - The network is always assumed to be hostile.
    - External and internal threats exist on the network at all times.
    - Network locality is not sufficient for deciding trust in a network.
    - Every device, user, and network flow is authenticated and authorized.
    - Policies must be dynamic and calculated from as many sources of data as possible.
- **Single Component**
    - In practice, the Zero Trust segmentation platform is a single component   in a single physical location. Because of performance, scalability, and physical limitations, an effective implementation is more likely to entail multiple instances distributed throughout an organization’s network. The solution also is called a “platform” to reflect that it is made up of multiple distinct (and potentially distributed) security technologies that operate as part of a holistic threat protection framework to reduce the attack surface and correlate information about discovered threats.
- **Management Infrastructure**
    - Centralized management capabilities are crucial to enabling efficient administration and ongoing monitoring, particularly for implementations involving multiple distributed Zero Trust segmentation platforms. A data acquisition network also provides a convenient way to supplement the native monitoring and analysis capabilities for a Zero Trust segmentation platform. Session logs that have been forwarded to a data acquisition network can then be processed by out-of-band analysis tools and technologies intended, for example, to enhance network visibility, detect unknown threats, or support compliance reporting.

Traditional security models identify areas where breaches and exploits may occur, the attack surface, and you attempt to secure the entire surface. Unfortunately, it is often difficult to identify the entire attack surface. Unauthorized applications, devices, and misconfigured infrastructure can expand that attack surface without your knowledge.

With the protect surface identified, you can identify how traffic moves across the organization in relation to the protect surface. Understanding who the users are, which applications they are using, and how they are connecting is the only way to determine and enforce policy that ensures secure access to your data. With an understanding of the interdependencies between the DAAS, infrastructure, services, and users, you should put controls in place as close to the protect surface as possible, creating a micro-perimeter around it. This micro-perimeter moves with the protect surface, wherever it goes.

In the Zero Trust model, only known and permitted traffic is granted access to the protect surface. A segmentation gateway, typically a next-generation firewall, controls this access. The segmentation gateway provides visibility into the traffic and users attempting to access the protect surface, enforces access control, and provides additional layers of inspection. Zero Trust policies provide granular control of the protect surface, making sure that users have access to the data and applications they need to perform their tasks but nothing more. This is known as least privilege access.

**Zero Trust Least Privilege Access Mode**
Additionally, to implement a Zero Trust least privilege access model in the network, the firewall must:
- Have visibility of and control over the applications and their functionality in the traffic.
- Be able to allow specific applications and block everything else
- Dynamically define access to sensitive applications and data based on a user's group membership.
- Dynamically define access from devices/device groups to sensitive applications and data and from users and user groups to specific devices
- Be able to validate a user's identity through authentication
- Dynamically define the resources that are associated with the sensitive data or application
- Control data by file type and content
- Zero trust segmentation platform
- **Trust zones**: Forrester Research refers to a trust zone as a micro core and perimeter (MCAP). A trust zone is a distinct pocket of infrastructure where the member resources not only operate at the same trust level but also share similar functionality. Functionality such as protocols and types of transactions must be shared in order to minimize the number of allowed pathways into and out of a given zone and, in turn, to minimize the potential for malicious insiders and other types of threats to gain unauthorized access to sensitive resources.
Remember, too, that a trust zone is not intended to be a “pocket of trust” where systems (and therefore threats) within the zone can communicate freely and directly with each other. For a full Zero Trust implementation, the network would be configured to ensure that all communications traffic – including traffic between devices in the same zone – is intermediated by the corresponding Zero Trust Segmentation Platform.

**Zero Trust Capabilities**
The core of any Zero Trust network security architecture is the Zero Trust Segmentation Platform, so you must choose the correct solution. Key criteria and capabilities to consider when selecting a Zero Trust Segmentation Platform include.

1. **Secure Access**: Consistent secure IPsec and SSL VPN connectivity is provided for all employees, partners, customers, and guests wherever they’re located (for example, at remote or branch offices, on the local network, or over the internet). Policies to determine which users and devices can access sensitive applications and data can be defined based on application, user, content, device, device state, and other criteria.
2. **Inspection of All Traffic**: Application identification accurately identifies and classifies all traffic, regardless of ports and protocols, and evasive tactics, such as port hopping or encryption. Application identification eliminates methods that malware may use to hide from detection and provides complete context into applications, associated content, and threats.
3. **Least Privileges Access Control**: The combination of application, user, and content identification delivers a positive control model that allows organizations to control interactions with resources based on an extensive range of business-relevant attributes, including the specific application and individual functions being used, user and group identity, and the specific types or pieces of data being accessed (such as credit card or Social Security numbers). The result is truly granular access control that safely enables the correct applications for the correct sets of users while automatically preventing unwanted, unauthorized, and potentially harmful traffic from gaining access to the network.
4. **Cyberthreat Protection**: A combination of anti-malware, intrusion prevention, and cyberthreat prevention technologies provides comprehensive protection against both known and unknown threats, including threats on mobile devices. Support for a closed-loop, highly integrated defense also ensures that inline enforcement devices and other components in the threat protection framework are automatically updated.
5. **Coverage for All Security Domains**: Virtual and hardware appliances establish consistent and cost-effective trust boundaries throughout an organization’s network, including in remote or branch offices, for mobile users, at the internet perimeter, in the cloud, at ingress points throughout the data center, and for individual areas wherever they might exist.

**Zero Trust Implementation**
Implementation of a Zero Trust network security model doesn’t require a major overhaul of an organization’s network and security infrastructure.

A Zero Trust design architecture can be implemented with only incremental modifications to the existing network, and implementation can be completely transparent to users. Advantages of such a flexible, non-disruptive deployment approach include minimizing the potential impact on operations and being able to spread the required investment and work effort over time.
- **Configure Listen-Only Mode**: To get started, security teams can configure a Zero Trust segmentation platform in listen-only  mode to obtain a detailed picture of traffic flows throughout the network, including where, when, and to what extent specific users are using specific applications and data resources.
- **Define Zero Trust Zones**: With a detailed understanding of the network traffic flows in the environment, the next step is to define trust zones and incrementally establish trust boundaries based on relative risk or sensitivity of the data involved. Security teams should deploy devices in appropriate locations to establish internal trust boundaries for defined trust zones. Then, they should configure enforcement and inspection policies to effectively put each trust boundary “online.”  
- **Establish Zero Trust Zones**: Next, security teams can progressively establish trust zones and boundaries for other segments of the computing environment based on their relative degree of risk. Examples of where secure trust zones can be established include IT management systems and networks, where a successful breach could lead to compromise of the entire network; partner resources and connections (business to business, or B2B); high-profile, customer-facing resources and connections (business to consumer, or B2C); branch offices in risky countries or regions, followed by all other branch offices; guest access networks (both wireless and wired); and campus networks.
- **Implement at Major Access Points**: Zero Trust principles and concepts must be implemented at major access points to the internet. Security teams will have to replace or augment legacy network security devices with a Zero Trust segmentation platform at this deployment stage to gain the capabilities and benefits of a Zero Trust security model.  

# Understanding Cybercrime and Security Threats
Cybercrime and security threats continue to evolve, challenging organizations to keep up as network boundaries and attack surfaces expand. Security breaches and intellectual property loss can have a huge impact on organizations.
## Security Approaches and Challenges
Current approaches to security, which focus mainly on detection and remediation, do not adequately address the growing volume and sophistication of attacks.
- **Automation and Big Data Analytics**: Cybercriminals leverage automation and big data analytics to execute massively scalable and increasingly effective attacks against their targets. They often share data and techniques with other threat actors to keep their approach ahead of point security products. Cybercriminals are not the only threat: Employees may often unknowingly violate corporate compliance and expose critical data in locations such as the public cloud.
- **Decentralization of IT Infrastructure**: With applications moving to the cloud, the decentralization of IT infrastructure, and the increased threat landscape, organizations have lost visibility and control.
- **Traditional Security Products**: Devices are proliferating and the network perimeter has all but disappeared, leaving enterprise security teams struggling to safely enable and protect their businesses, customers, and users. With new threats growing in number and sophistication,  organizations are finding that traditional security products and approaches are less and less capable of protecting their networks against today’s advanced cyberattacks.
- **Complex Network**: Application development and IT operations teams are accelerating the delivery of new applications to drive business growth by adopting DevOps tools and methodologies, cloud and container technologies, big data analytics, and automation and orchestration. Meanwhile, applications are increasingly accessible. The result is an incredibly complex network that introduces significant business risk. Organizations must minimize this risk without slowing down the business.

## Prevention Architecture
The product portfolio's prevention architecture allows organizations to reduce threat exposure by first enabling applications for all users or devices in any location and then preventing threats within application flows, tying application use to user identities across physical, cloud-based, and software-as-a-service (SaaS) environments.
- **Provide Full Visibility**: To understand the full context of an attack, the portfolio provides visibility of all users and devices across the organization’s network, endpoint, cloud, and SaaS applications.
- **Reduce the Attack Surface**: Best-of-breed technologies that are natively integrated provide a prevention architecture that inherently reduces the attack surface. This type of architecture allows organizations to exert positive control based on applications, users, and content, with support for open communication, orchestration, and visibility.
- **Prevent All Known Threats, Fast**: A coordinated security platform accounts for the full scope of an attack across all security controls, enabling organizations to quickly identify and block known threats.
- **Detect and Prevent New, Unknown Threats with Automation**: Building security that detects threats and requires a manual response is too little, too late. Automated creation and delivery of near-real-time protections against new threats enables dynamic policy updates, which allow enterprises to scale defenses with technology, rather than people.

## Prevention-First Architecture
Palo Alto Networks is helping to address the world’s greatest security challenges with continuous innovation that seizes the latest breakthroughs in artificial intelligence, analytics, automation, and orchestration. By delivering an integrated platform and empowering a growing ecosystem of partners, Palo Alto Networks is at the forefront of protecting tens of thousands of organizations across clouds, networks, and mobile devices.

The Palo Alto Networks portfolio of security technologies and solutions addresses three essential areas of cybersecurity strategy.
1. **Secure the Enterprise with Strata**
Prevent attacks with the industry-leading network security suite, which enables organizations to embrace network transformation while consistently securing users, applications, and data, no matter where they reside.
    - **PAN‑OS** software runs Palo Alto Networks next-generation firewalls. PAN-OS natively uses key technologies (App‑ID, Content‑ID, Device-ID, and User‑ID) to provide complete visibility and control of applications in use across all users, devices, and locations all the time. Inline ML and application and threat signatures automatically reprogram the firewall with the latest intelligence so allowed traffic is free of known and unknown threats.
    - **Panorama** network security management enables centralized control, log collection, and policy workflow automation across all next-generation firewalls (scalable to tens of thousands of firewalls) from a single pane of glass.
    - **Cloud-Based Subscription Services**, including DNS Security, URL Filtering, Threat Prevention, and WildFire® malware prevention, deliver real-time advanced predictive analytics, AI and machine learning, exploit/malware/C2 threat protection, and global threat intelligence to the Palo Alto Networks Security Operating Platform.
2. **Secure the Cloud with Prisma**: The Prisma suite secures public cloud environments, SaaS applications, internet access, mobile users, and remote locations through a cloud-delivered architecture. It is a comprehensive suite of security services to effectively predict, prevent, detect, and automatically respond to security and compliance risks without creating friction for users, developers, and security and network administrators.
    - **Prisma Cloud** is the industry’s most comprehensive threat protection, governance, and compliance offering. It dynamically discovers cloud resources and sensitive data across AWS, GCP, and Azure to detect risky configurations and identify network threats, suspicious user behavior, malware, data leakage, and host vulnerabilities. It eliminates blind spots across cloud environments and provides continuous protection with a combination of rule-based security policies and class-leading machine learning.
    - **Prisma Access** is a Secure Access Service Edge (SASE) platform that helps organizations deliver consistent security to their remote networks and mobile users. It’s a generational step forward in cloud security, using a cloud-delivered architecture to connect all users to all applications. All of an organization's users, whether at headquarters, in branch offices, or on the road, connect to Prisma Access to safely use cloud and data center applications, as well as the internet. Prisma Access consistently inspects all traffic across all ports and provides bidirectional software-defined wide-area networking (SD-WAN) to enable branch-to-branch and branch-to-headquarters traffic.
    - **Prisma SaaS** functions as a multimode cloud access security broker (CASB), offering inline and API-based protection working together to minimize the range of cloud risks that can lead to breaches. With a fully cloud-delivered approach to CASB, organizations can secure their SaaS applications through the use of inline protections to safeguard inline traffic with deep application visibility, segmentation, secure access, and threat prevention, as well as API-based protections to connect directly to SaaS applications for data classification, data loss prevention, and threat detection.
3. **Secure the Future with Cortex**: Cortex is designed to simplify security operations and considerably improve outcomes. Cortex is enabled by the Cortex Data Lake, where customers can securely and privately store and analyze large amounts of data that is normalized for advanced AI and machine learning to find threats and orchestrate responses quickly.
    - **Cortex XDR** breaks the silos of traditional detection and response by natively integrating network, endpoint, and cloud data to stop sophisticated attacks. Taking advantage of machine learning and AI models across all data sources, it identifies unknown and highly evasive threats from managed and unmanaged devices.
    - **Cortex XSOAR** is the only security orchestration, automation, and response (SOAR) platform that combines security orchestration, incident management, and interactive investigation to serve security teams across the incident lifecycle.
    - **Cortex Data Lake** enables AI-based innovations for cybersecurity with the industry’s only approach to normalizing an enterprise’s data. It automatically collects, integrates, and normalizes data across an organization's security infrastructure. The cloud-based service is ready to scale from the start, eliminating the need for local compute or storage, providing assurance in the security and privacy of data.
    - **AutoFocus** contextual threat intelligence service speeds an organization's ability to analyze threats and respond to cyberattacks. Instant access to community-based threat data from WildFire, enhanced with deep context and attribution from the Palo Alto Networks Unit 42 threat research team, saves time. Security teams get detailed insight into attacks with prebuilt Unit 42 tags that identify malware families, adversaries, campaigns, malicious behaviors, and exploits without the need for a dedicated research team.







