# Azure Active Directory Concepts
| Concept | Description |
| ----------- | ----------- |
| Identity | An identity is an object that can be authenticated. The identity can be a user with a username and password. Identities can also be applications or other servers that require authentication by using secret keys or certificates. Azure AD is the underlying product that provides the identity service. |
| Account | An account is an identity that has data associated with it. To have an account, you must first have a valid identity. You can't have an account without an identity. |
| Azure AD account | An Azure AD account is an identity that's created through Azure AD or another Microsoft cloud service, such as Microsoft 365. Identities are stored in Azure AD and are accessible to your organization's cloud service subscriptions. The Azure AD account is also called a work or school account. |
| Azure tenant (directory) | An Azure tenant is a single dedicated and trusted instance of Azure AD. Each tenant (also called a directory) represents a single organization. When your organization signs up for a Microsoft cloud service subscription, a new tenant is automatically created. Because each tenant is a dedicated and trusted instance of Azure AD, you can create multiple tenants or instances. |
| Azure subscription | An Azure subscription is used to pay for Azure cloud services. A subscription is linked to a credit card. Each subscription is joined to a single tenant. You can have multiple subscriptions. |

## Things to consider when using Azure AD rather than AD DS

Azure AD is similar to AD DS, but there are significant differences. It's important to understand that using Azure AD for your configuration is different from deploying an Active Directory domain controller on an Azure virtual machine and then adding it to your on-premises domain.

As you plan your identity strategy, consider the following characteristics that distinguish Azure AD from AD DS.

- **Identity solution:** AD DS is primarily a directory service, while Azure AD is a full identity solution. Azure AD is designed for internet-based applications that use HTTP and HTTPS communications. The features and capabilities of Azure AD support target strong identity management.

- **REST API queries:** Azure AD is based on HTTP and HTTPS protocols. Azure AD tenants can't be queried by using LDAP. Azure AD uses the REST API over HTTP and HTTPS.

- **Communication protocols:** Because Azure AD is based on HTTP and HTTPS, it doesn't use Kerberos authentication. Azure AD implements HTTP and HTTPS protocols, such as SAML, WS-Federation, and OpenID Connect for authentication (and OAuth for authorization).

- **Federation services:** Azure AD includes federation services, and many third-party services like Facebook.

- **Flat structure:** Azure AD users and groups are created in a flat structure. There are no organizational units (OUs) or group policy objects (GPOs).

- **Managed service:** Azure AD is a managed service. You manage only users, groups, and policies. If you deploy AD DS with virtual machines by using Azure, you manage many other tasks, including deployment, configuration, virtual machines, patching, and other backend processes.

## Azure AD Editions

| Feature | Free | Microsoft 365 Apps | Premium P1 | Premium P2 |
| ----------- | :-----------: | :-----------: | :-----------: | :-----------: |
| Directory Objects | 500K | Unlimited | Unlimited | Unlimited |
| Single Sign-on | Unlimited | Unlimited | Unlimited | Unlimited |
| Core Identity and Access Management | X | X | X | X |
| Business-to-business Collaboration | X | X | X | X |
| Identity and Access Management for Microsoft 365 apps |  | X | X | X |
| Premium Features | | | X | X |
| Hybrid Identities | | | X | X |
| Advanced Group Access Management |  | | X | X |
| Conditional Access |  | | X | X |
| Identity Protection | | | | X |
| Identity Governance |  | | | X |

[Azure AD Pricing](https://azure.microsoft.com/pricing/details/active-directory)

Every user who wants access to Azure resources needs an Azure user account. A user account has all the information required to authenticate the user during the sign-in process. Azure Active Directory (Azure AD) supports three types of user accounts. 
| User Account | Description |
| ----------- | ----------- |
| Cloud identity | A user account with a cloud identity is defined only in Azure AD. This type of user account includes administrator accounts and users who are managed as part of your organization. A cloud identity can be for user accounts defined in your Azure AD organization, and also for user accounts defined in an external Azure AD instance. When a cloud identity is removed from the primary directory, the user account is deleted. |
| Directory-synchronized identity | User accounts that have a directory-synchronized identity are defined in an on-premises Active Directory. A synchronization activity occurs via Azure AD Connect to bring these user accounts in to Azure. The source for these accounts is Windows Server Active Directory. |
| Guest user | Guest user accounts are defined outside Azure. Examples include user accounts from other cloud providers, and Microsoft accounts like an Xbox LIVE account. The source for guest user accounts is Invited user. Guest user accounts are useful when external vendors or contractors need access to your Azure resources. |

## Manage identities and governance in Azure
### Management Groups
Think of management groups as containers to manage access, policy, and compliance across your subscriptions.​

- By default, all new subscriptions are placed under the top-level management group, or root group.
- All subscriptions within a management group automatically inherit the conditions applied to that management group.
- A management group tree can support up to six levels of depth.
- Azure role-based access control authorization for management group operations isn't enabled by default.

#### Things to consider when using management groups

Review the following ways you can use management groups in Azure Policy to manage your subscriptions:

- **Consider custom hierarchies and groups.** Align your Azure subscriptions by using custom hierarchies and grouping that meet your company's organizational structure and business scenarios. You can use management groups to target policies and spending budgets across subscriptions.

- **Consider policy inheritance.** Control the hierarchical inheritance of access and privileges in policy definitions. All subscriptions within a management group inherit the conditions applied to the management group. You can apply policies to a management group to limit the regions available for creating virtual machines (VMs). The policy can be applied to all management groups, subscriptions, and resources under the initial management group, to ensure VMs are created only in the specified regions.

- **Consider compliance rules.** Organize your subscriptions into management groups to help meet compliance rules for individual departments and teams.

- **Consider cost reporting.** Use management groups to do cost reporting by department or for specific business scenarios. You can use management groups to report on budget details across subscriptions.

[Management Group Hierarchy](/images/azure/management_group.png)

### Azure Policy
Azure Policy is a service in Azure that enables you to create, assign, and manage policies to control or audit your resources. These policies enforce different rules over your resource configurations so the configurations stay compliant with corporate standards. You apply the policies to your resources by using management groups.

### Things to consider when using Azure Policy

Review the following scenarios for using Azure Policy. Consider how you can implement the service in your organization.

- **Consider deployable resources.** Specify the resource types that your organization can deploy by using Azure Policy. You can specify the set of virtual machine SKUs that your organization can deploy.

- **Consider location restrictions.** Restrict the locations your users can specify when deploying resources. You can choose the geographic locations or regions that are available to your organization.

- **Consider rules enforcement.** Enforce compliance rules and configuration options to help manage your resources and user options. You can enforce a required tag on resources and define the allowed values.

- **Consider inventory audits.** Use Azure Policy with Azure Backup service on your VMs and run inventory audits.

#### Working with Azure Policies
There are 4 basic steps to create and work with policy definitions in Azure Policy.

1. Create policy definitions
A policy definition expresses a condition to evaluate and the actions to perform when the condition is met. You can create your own policy definitions, or choose from built-in definitions in Azure Policy. You can create a policy definition to prevent VMs in your organization from being deployed, if they're exposed to a public IP address.

2. Create an initiative definition
An initiative definition is a set of policy definitions that help you track your resource compliance state to meet a larger goal. You can create your own initiative definitions, or use built-in definitions in Azure Policy. You can use an initiative definition to ensure resources are compliant with security regulations.

3. Scope the initiative definition
Azure Policy lets you control how your initiative definitions are applied to resources in your organization. You can limit the scope of an initiative definition to specific management groups, subscriptions, or resource groups.

4. Determine compliance
After you assign an initiative definition, you can evaluate the state of compliance for all your resources. Individual resources, resource groups, and subscriptions within a scope can be exempted from having the policy rules affect it. Exclusions are handled individually for each assignment.