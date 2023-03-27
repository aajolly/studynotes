![Governance_at_scale_example1](/images/governance/governance_org_hierarchy.png)
![Governance_at_scale_example2](/images/governance/governance_sec_%26_compliance.png)
![Governance_at_scale_example3](/images/governance/governance_budget.png)

Governance at scale is a process, or lifecycle, that includes implementing, provisioning, and operations. 

**Implementing**: helps an organization innovate faster and perform daily activities in an agile manner. You must focus on what matters—application solutions or services-while maintaining a strong security posture.

![governance_at_scale_Implement](/images/governance/governance_at_scale_Implement.png)

From a governance at scale perspective, implement refers to creating and automating a landing zone created on Day 1.

It's always Day 1

The expression "It’s always Day One" means that there is always something new, disruptive, or innovative to manage. 

Here are some common tasks:
1. Deploy a new workload in AWS
2. Provision a resource for a workload
3. Start a business unit journey in AWS, the right way.
4. On-board a new team member

All the above tasks must be performed while meeting security and compliance requirements.

### One account (or two) isn't enough

You face challenges when you try to isolate, track, and monitor one or two accounts. For example:
- Sharing an account across different teams can cause conflict in visibility and accountability.
- Security requirements can differ across teams
- You cannot separate items at a billing level with one account. Multiple accounts can identity the actual spending per business unit, team, or workload.

A well-architected AWS environment should implement a multi-account approach.

AWS Control Tower can be used to manage governance at scale within AWS. It enables the following:
1. Setup a landing zone
2. Establish Guardrails
3. Automate compliant account provisioning: You can automate account provisioning workflow with Account Factory. After your landing zone is ready, you must still accommodate new workloads and new teams. AWS Control Tower simplifies the cloud onboarding process.
4. Centralise identity and access: Centralized identity and access can centralize access and identity management using AWS SSO. Accounts follow a standard best practice and integrate users with an identity system, such as a managed Azure Active Directory (Azure AD), Okta, or both.

A centralized dashboard gives you visibility into your OUs, accounts, and guardrails. If a guardrail is violated, you need to be notified, and you want to correct the issue. Then, you need to manage your environment in a continuous cycle.

### AWS Control Tower (CT)
AWS Control Tower provisions two core accounts:

1. The **audit** account can access audit information made available by AWS Control Tower. You can use this account to grant access to third-party auditing companies for their reviews.
2. The **log archive** account is for accessing all logging information across managed accounts in the landing zone OUs.

Before AWS CT sets up the landing zone, it automatically runs a series of pre-launch checks in your account. There's no action required on your part for these checks, which ensure that your management account is ready for the changes that establish your landing zone. Refer to the [documentation](https://docs.aws.amazon.com/controltower/latest/userguide/getting-started-prereqs.html) for more details.

![Control_Tower_LZ_Reference_Architecture](/images/governance/landing_zone_reference_arch.png)

1. Management Account: Apply guardrails at OU level and not the account level.
2. Core & Custom OUs: Default org structure creates these two OUs. The **Core OU** contains the log archive, the audit accounts, and the resources they own. The **Custom OU** is a child OU that contains your member accounts, where end users can perform work on AWS resources.
3. Single Sign-On: By default AWS CT uses built-in AWS SSO for centralised account federation. AWS SSO can be integrated with Azure AD, OKTA etc.
4. Account Factory: Console based product part of AWS Service Catalog. Use it for automated provisioning of AWS accounts and applying account baselines and guardrails.
5. Shared Accounts: Auditing & log archive. Use the auditing account as the management account for all security related services (SecurityHub, GuardDuty etc)
6. Guardrails: Set rules using AWS Config
7. Production Infrastructure OU: Hosts additional accounts, such as shared services account and network account.
8. Configure remaining accounts as member accounts for GuardDuty and SecurityHub.

### Automating Compliant Account Provisioning
Account Factory provides automated account vending through AWS Account vending machine, which provisions and automatically configures new accounts, and prevents account sprawl. Vending machine lets you create preapproved baselines and configuration options for AWS accounts. You can create accounts with AWS Control Tower, AWS Service Catalog, and AWS Service Catalog API.

**AWS CT**: As an administrator, you can use the enroll account option to create a member account in AWS Control Tower.

**AWS Service Catalog**: AWS Control Tower landing zone setup creates an AWS Control Tower Account Factory portfolio, a new product portfolio that contains a product called AWS Control Tower Factory. It provisions a new account managed by AWS Control Tower. It also provides a network baseline configuration.

**AWS Service Catalog API**: You can programmatically call or use AWS Service Catalog API from the landing zone’s management account.

#### Preventive guardrails
Preventive guardrail behavior ensures that your accounts remain compliant, because it disallows actions that lead to policy violations. The status of a preventive guardrail is either enforced or not turned on. Preventive guardrails are supported in all AWS Regions. Implement preventive guardrails using service control policies (SCPs). 

#### Detective guardrails
The detective guardrail discovers noncompliant resources within your accounts, such as policy violations, and provides alerts through the dashboard. The status of a detective guardrail is either clear, in violation, or not turned on. Detective guardrails apply only in those AWS Regions supported by AWS Control Tower. Apply detective guardrails using AWS Config rules and AWS Lambda functions.

### Establishing Guardrails 

#### Guardrails guidance
Guardrail guidance refers to the recommended practice of applying each guardrail to your OUs. The guidance for a guardrail is independent of it being preventive or detective. AWS Control Tower provides three categories of guidance: mandatory, strongly recommended, and elective.

- **Mandatory guardrails**: Always enforced.
- **Strongly recommended guardrails**: Based on some common best practices for well-architected, multi-account environments.
- **Elective guardrails** let customers track or lock down actions that are commonly restricted in an AWS enterprise environment.

### Provision
The provision aspect of governance at scale—how developers can work in a self-service and secure fashion.

Developers must move from concept to production as fast as possible. The ability to move fast has multiple dependencies in the development lifecycle.

1. **Compliance**: Ensure blockers are cleared.
2. **Best practices**: Avoid scaling and security issues.
3. **Agility**: Add value faster.
4. **Standardization**: Use approved processes and assets.

#### Developer challenges
Without governance at scale, developers might run into many, or all, of these challenges daily.
The typical challenges are processes, costs and best practice implementations.
Process related challenges: It boils down to lack of self-service processes that require developers to follow manual processes like requesting another team for resources.
Decentralized Governance makes best practices difficult to implement. Example: tagging and naming conventions. 
Developers need to address these challenges themselves without depending on other teams or 3rd party.

#### Goals of a self-service solution
Think about the goals that a self-service solution can help you accomplish. 

1. The right people have access to the right services.
2. Services are used in accordance with an organization’s policies, security rules, and compliance requirements.
3. Everything, including patching, is tracked to monitor usage, product version, and costs. 

AWS Service Catalog is a service that can help in creating a self-service solution.

### Operate
Having preventive controls in place to ensure that resources aren’t misconfigured and out of compliance is great. However, how to ensure that those resources stay properly configured, secured, and compliant? Can you identify and detect undesired changes on their configuration and compliance status? And after you detect that, what can you do about it?

To start, you will explore how you can operate with agility—and stay in control.

#### Four aspects of agility and control
1. Monitor resources and applications.
2. Audit resource configurations, user access, and policy enforcement.
3. Take operational action on resources.
4. Analyze, improve efficiency, and manage security posture.

AWS Management Services
AWS provides a suite of services that customers can use to implement governance across their solution.

![management_services](/images/governance/mgmt_services.png)

### Key Pillars of Effective Governance
#### Inventory and configuration management
For the inventory and configuration management pillar, customers answer questions like:
1. What is currently in my inventory? (Real-time resource inventory)
2. What is the latest configuration state of my resources? (Configuration snapshot)
3. What relationships exist between my resources? (Resource relationships)
4. What configuration changes occurred on my critical workload in a specific time period? (Configuration history)
5. Which EC2 instances are built on top of a certain machine image? (For example, AMI-234314)

**Note**: This information is valuable during audits and for investigating security breaches. For example, an inventory and compliance system can detect which online EC2 instances might have security vulnerabilities because they haven’t been patched.

#### Configuration compliance management
For the configuration compliance management pillar, customers answer questions like:
1. Are my resources still properly configured? (Best-practice configuration checks)
2. Do my resources comply with regulatory requirements? (For example, PCI or HIPAA)
3. How do I ensure compliance? (Checking for policy violations immediately after a configuration change)
4. How can I get notified in real time if certain resources go out of compliance? (Real-time compliance change notifications)

In summary, an effective governance framework supports governance initiatives by providing accurate configuration information to assist with decision making. It also minimizes the number of quality and compliance issues caused by incorrect or inaccurate configuration.

#### Governance lifecycle with services
These services work together and play a crucial role in the governance at scale framework. 

**AWS Config**: With AWS Config, you can assess, audit, and evaluate your AWS configurations. AWS Config monitors and records AWS resource configurations and automates the evaluation of recorded configurations against desired configurations.

There are two types of guardrails: preventive and detective. AWS Config rules are added to detective guardrails.

**Systems Manager**: Systems Manager gives you visibility and control of your infrastructure on AWS. It provides a unified user interface, so you can view operational data from multiple AWS services. It also lets you automate operational tasks across your AWS resources.

Here are some important features from a governance perspective
1. It lets you centralize operational data from multiple AWS services and automate tasks across AWS resources.
2. __Built-in Insights__: A dashboard provides a way for you to access other AWS services and data from the context of a resource group. You can configure your own operations dashboard by using existing CloudWatch metrics.
3. __Software inventory tracking__: You can build your software catalog using the inventory feature and specify characteristics of your infrastructure, such as OS version, application, and installed software versions. You can also track folders, files, and permissions.
4. __Security and compliance__: After your build this software, you can define compliant behaviors and implement compliance rules. Systems manager scans instances against patch, configuration, and custom policies. You can define patch baselines, maintain up-to-date antivirus definitions, and enforce firewall policies. You can also remotely manage your servers at scale without manually logging into each server.
5. __Centralized Configuration Management__: It also provides a centralised store to manage configuration data, whether it's plain text, such as database strings, or secrets, such as passwords. This lets you to separate secrets and configuration data from code.
6. __3rd Party tools integration__: ITSM tools, such as ServiceNow, can connect with Systems Manager to help ITSM platform users manage AWS and 3rd party resources. The AWS service management connector helps ITSM administrators improve governance and provisioned AWS and 3rd party products.

**GuardDuty**: It protects AWS accounts, workloads, and data with intelligent threat detection and monitoring. It is a threat detection service that monitors for malicious activity and unauthorized behavior to protect AWS accounts, workloads, and data stored in Amazon Simple Storage Service (Amazon S3). 

Here are some important features from a governance perspective
1. __Monitoring & Threat Detection__: GuardDuty identifies threats by monitoring network activity, data access patterns, and account behavior in AWS. It is integrated with up-to-date threat intelligence feeds from AWS, such as CrowdStrike, and Proofpoint. Threat intelligence, coupled with machine learning and behavior models, help you detect activity such as cryptocurrency mining and credential compromise behavior. You can also detect unauthorized and unusual data access, communication with known command-and-control servers, or API calls from known malicious IP addresses.
2. __Security Automation__:GuardDuty automates how to respond to threats, reducing remediation and recovery time. GuardDuty security findings are informative and actionable for security operations. The findings include the affected resource’s details and attacker information, such as IP address and geolocation.
3. __Multi-account support__: GuardDuty provides multi-account support using Organizations, so customers can turn on GuardDuty across existing and new accounts. Security teams can aggregate an organization's findings across accounts into a single GuardDuty administrator account for easier management. The aggregated findings are also available through Amazon CloudWatch Events, facilitating integration with existing enterprise event management systems.
4. __Simplified Management__: When GuardDuty is activated, it immediately starts to analyze streams of account and network activity in near real time and at scale. You have no additional security software, sensors, or network appliances to deploy or manage. Threat intelligence is preintegrated into the service and is regularly updated and maintained.

**Security Hub**: It is the compliance and security center for AWS customers. 

Here are some important features from a governance perspective
1. __Security & Compliance__: Security Hub helps answer fundamental security and compliance questions, such as, “Am I compliant?” or “Am I secure?” 
2. __Findings in one location__: With Security Hub, you can manage security and compliance findings in one location, reducing the time spent wrangling data from different locations in the console.
3. __Multi-account support__
4. __Partner Integrations__: Security Hub integrates with 28 Partner integrations, three automated AWS integrations, and more than 25 out-of-the-box AWS Config rules.
5. __Industry standards & best practices__: Security Hub automates continuous account- and resource-level configuration and security checks using industry standards and best practices.
6. __Automated Compliance__: Security Hub automates compliance. If any accounts or resources deviate from a best practice, Security Hub flags the problem and recommends remediation steps.

#### Summary
![summary_services](/images/governance/summary_aws_services.png)


