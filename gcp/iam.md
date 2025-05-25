# Google Cloud IAM (Identity and Access Management)
## Hierarchy
### IAM & Resource Hierarchy in Google Cloud
| **Concept** | **Description** |
| :------- | :--------- |
| **Resource Hierarchy** | Organization → Folder → Project → Resources |
| **IAM Policy Inheritance** | Policies are inherited top-down (Org → Folder → Project → Resource) |
| **IAM Deny Policies** | Override allow policies and explicitly block access |
| **Billing Flow** | Accumulates bottom-up (Resource → Project → Billing Account → Organization) |
| **Project Identifiers** | Project Name (human-readable), Project ID (unique), Project Number (system-generated) |
| **Resource Types** | Global (e.g., networks), Regional (e.g., IPs), Zonal (e.g., VMs, disks) |

#### Key Capabilities of Google Cloud Resource Manager
- **Centralized Policy Management**: IAM roles and policies can be applied at any level and inherited by child resources.
- **Project Isolation**: Each project is a boundary for billing, APIs, and IAM.
- **Fine-Grained Access Control**: Use IAM roles and conditions to control access.
- **Auditability**: All changes and access are logged via Cloud Audit Logs.

### Organization Node
- **Root of the GCP hierarchy**: Represents the entire company.
- **Key Roles**:
    - **Organization Admin**: Full administrative access across all resources; ideal for auditing.
    - **Project Creator**: Can create projects within the organization; role can be inherited by all projects.
- **Linked to**: G Suite or Cloud Identity Account.
- **Super Admins**: Have overarching control and are notified when an organization resource is provisioned.

### Responsibilities
- **G Suite/Cloud Identity Super Admin**:
    - Assigns organization admin roles.
    - Acts as recovery contact.
    - Manages lifecycle of the G Suite/Cloud Identity account and organization resource.

- **Organization Admin**:
    - Defines IAM policies.
    - Structures the resource hierarchy.
    - Delegates responsibilities (e.g., billing, networking).
    - Needs additional roles to perform actions like folder creation (least privilege principle).

### Folder Structure
- **Folders = Sub-organizations**:
    - Used to model departments, teams, or applications.
    - Can contain projects and other folders.
    - Enable isolation and delegation of access.
    - Example: Department → Team → Application.

### Roles and Inheritance
- **Organization Node**:
    - **Viewer**: View access to all resources.
- **Folder Node**:
    - **Admin**: Full control over folders.
    - **Creator**: Can browse and create folders.
    - **Viewer**: Can view folders and projects.
- **Project Node**:
    - **Creator**: Can create and own new projects.
    - **Deleter**: Can delete projects.

## Purpose of IAM
- Controls who can do what on which resources in a Google Cloud environment.
- Helps enforce least-privilege access across folders, projects, and resources.

## Key Concepts
- **Principal**: The “who” in IAM. Can be:
    - Google account
    - Google group
    - Service account
    - Cloud Identity domain
- **Role**: Defines the “can do what.” A collection of permissions.
- **Policy**: Binds a principal to a role on a resource.

## IAM Policy Inheritance
- Policies apply to the resource and all its children in the hierarchy.
- Deny policies override allow policies and are also inherited.

### IAM Policies and Resource Hierarchy
- **Hierarchy Structure**:
    - **Organization Node**: Root of the hierarchy, represents the company.
    - **Folders**: Represent departments, children of the organization.
    - **Projects**: Represent trust boundaries, children of folders.
    - **Resources**: Individual services, children of projects.
- **Inheritance**:
    - Roles granted at the **organization** level apply to all resources beneath it.
    - Roles at the **folder** level apply to all resources within that folder.
    - Services within the same **project** share the same default trust level.
![iam-policy-inheritence](/images/gcp/iam-inheritence.png)

## Types of IAM Roles
### Basic Roles
Broad access across all resources in a project. Includes:
- **Viewer**: Read-only access
- **Editor**: Read/write access
- **Owner**: Full access + manage roles/billing
- **Billing Admin**: Manage billing only
![iam-roles](/images/gcp/iam-roles.png)

### Predefined Roles
Predefined roles are fine-grained, service-specific roles created and maintained by Google. They are designed to grant only the permissions required to perform specific tasks, aligning with the principle of least privilege.

Unlike basic roles (Owner, Editor, Viewer), which are broad and apply across all services, predefined roles are narrowly scoped and tailored to individual Google Cloud services like Compute Engine, Cloud Storage, BigQuery, etc.

#### Key Characteristics
| **Feature** | **Description** |
| :----- | :------ |
| Granularity | Predefined roles include only the permissions necessary for specific tasks. |
| Service-Specific | Each role is tied to a particular Google Cloud service (e.g., roles/compute.instanceAdmin). |
| Managed by Google | Google updates these roles as services evolve, ensuring they remain secure and functional. |
| Assignable Scope | Can be assigned at the organization, folder, or project level. |

##### Examples of Predefined Roles
| **Role Name** | **Service** | **Description** |
| :------ | :------ | :------- |
| `roles/compute.instanceAdmin` | Compute Engine | Full control over VM instances, but not over networks or firewalls. |
| `roles/storage.objectViewer` | Cloud Storage | Read-only access to objects in a bucket. |
| `roles/bigquery.dataEditor` | BigQuery | Read and write access to datasets and tables. |
| `roles/pubsub.publisher` | Pub/Sub | Permission to publish messages to a topic. |

##### Why Use Predefined Roles?
- **Security**: Reduces risk by avoiding over-permissioning.
- **Simplicity**: Easier to manage than custom roles for common use cases.
- **Compliance**: Helps meet regulatory requirements by limiting access.

### Custom Roles	
User-defined roles with specific permissions. Useful for least-privilege models.
> __**NOTE**__
> - Must be managed manually
> - Can only be applied at project or organization level (not folder level)

### When to Use Predefined vs. Custom Roles
| **Use Case** | **Recommended Role Type** |
| :----- | :----- |
| Common tasks with standard permissions | Predefined Role |
| Unique job functions requiring tailored permissions | Custom Role |
| Broad administrative access | Basic Role (with caution) |

# What Are Service Accounts in Google Cloud?
A service account is a special type of Google identity used by applications, virtual machines (VMs), and other compute workloads to authenticate and access Google Cloud services—without human intervention.

## Key Characteristics
- Identified by an email-like name (e.g., `my-service-account@project-id.iam.gserviceaccount.com`)
- Uses **cryptographic keys** (not passwords) for authentication
- Can be assigned **IAM roles** to define what it can do
- Can also have **IAM policies** applied to it, just like any other resource

## How They Work (with Example)
Imagine a VM running an app that needs to write to Cloud Storage. Instead of embedding user credentials or manually granting access:
- You assign a service account to the VM
- That service account has the necessary permissions (e.g., `roles/storage.objectAdmin`)
- The app can now securely access Cloud Storage using the service account’s identity

This is similar to how AWS EC2 instances assume IAM roles to access S3 or DynamoDB.

## Service Account Permissions
There are two layers of permissions:
1. **What the service account can do** (e.g., access Cloud Storage)
2. **Who can use or manage the service account** (e.g., Alice can impersonate or manage it)

| **Role** | **Purpose** |
| :---- | :----- |
| `roles/iam.serviceAccountUser` | Allows a user to impersonate a service account |
| `roles/iam.serviceAccountAdmin` | Full control over the service account |
| `roles/iam.serviceAccountTokenCreator` | Allows creation of OAuth2 tokens for the service account |

In essence, Google Service Accounts ≈ AWS IAM Roles used by services like EC2 or Lambda 2.

## Best Practices
- Use **least privilege**, only assign the permissions needed
- Rotate keys or use **short-lived tokens** where possible
- Monitor usage and audit access via **Cloud Audit Logs**
- Avoid assigning service account keys unless absolutely necessary

# What Is Google Cloud Identity?
**Google Cloud Identity** is a cloud-based identity and access management (IAM) platform that allows organizations to centrally manage users, groups, and devices across Google Cloud and other integrated services. It’s especially useful for organizations transitioning from ad hoc Gmail-based access to a more secure, scalable identity model.

## Key Capabilities
| **Feature** | **Description** |
| :------ | :------ |
| Centralized Identity Management | Admins can manage users and groups via the Google Admin Console, integrating with existing directories like Active Directory or LDAP. |
| Access Control | Policies can be enforced to control access to Google Cloud resources, apps, and services. |
| Account Lifecycle Management | When a user leaves the organization, their access can be revoked immediately from a central console. |
| Device Management	| Premium edition includes mobile device management (MDM) for enforcing security policies on endpoints. |
| SSO & MFA | Supports Single Sign-On (SSO) and Multi-Factor Authentication (MFA) for enhanced security.
| Editions | Available in Free and Premium tiers. Premium includes advanced security and device management features. |

## Example Use Case
Let’s say your team initially used personal Gmail accounts and Google Groups to collaborate. As the team grows, this becomes unmanageable—especially when someone leaves. With Cloud Identity:
- You can centrally manage all user accounts.
- Use existing credentials from Active Directory.
- Disable access instantly when someone exits the company.

## Google Cloud Identity vs AWS IAM Identity Center
| **Feature** | **Google Cloud Identity** | **AWS IAM Identity Center** |
| :------ | :--------- | :-------- |
| Primary Focus | Identity + Endpoint Management | Identity Management only |
| SSO Support | Yes (SAML, OIDC, LDAP) | Yes (SAML, SCIM) |
| Device Management | Yes (Premium edition) | No |
| Integration | Google Workspace, GCP, 3rd-party apps | AWS services, 3rd-party apps |
| Directory Sync | AD, LDAP, Google Workspace | AWS Directory Service, 3rd-party IdPs |
| Pricing | Free & Premium | Included with AWS Organizations |

# Interacting with Google Cloud
There are four main ways to access and interact with Google Cloud:
1. Google Cloud Console (Web UI)
**What it is**: A graphical user interface (GUI) for managing Google Cloud resources.
**Key Features**:
- Deploy, scale, and troubleshoot resources.
- Visual dashboards for monitoring and budgeting.
- SSH access to VM instances directly from the browser.
- Search bar to quickly locate resources.

2. Google Cloud SDK & Cloud Shell
**Google Cloud SDK**
- A downloadable toolkit that includes:
    - gcloud: CLI for managing Google Cloud services.
    - bq: CLI for BigQuery.
    - gsutil: CLI for Cloud Storage.
Installed tools reside in the bin directory.
**Cloud Shell**
A browser-based, Debian VM with:
    - Pre-installed SDK tools.
    - 5 GB persistent home directory.
    - Fully authenticated access to your Google Cloud environment.

3. APIs
**What it is**: RESTful APIs for programmatic access to Google Cloud services.
**Tools**:
- Google APIs Explorer: Try APIs interactively.
- Client Libraries: Available in Java, Python, Go, Node.js, C#, PHP, Ruby, and C++.
- Use Case: Automate infrastructure, integrate with apps, or build custom tools.

4. Google Cloud Mobile App
**What it is**: A mobile app for managing cloud resources on the go.
**Capabilities**:
- Start/stop Compute Engine and Cloud SQL instances.
- SSH into VMs.
- View logs and errors.
- Monitor billing and receive alerts.
- Visualize metrics like CPU usage and network traffic.
- Incident management and traffic splitting for App Engine.

# Billing Console Overview
- Accessed via Navigation Menu → Billing in the GCP Console.
- Shows monthly consumption, credits, and billing accounts.
- You can manage payment methods, payment profiles, and transactions.

## Budgets and Alerts
- Navigate to Budgets & Alerts → Create Budget.
- Steps to create a budget:
    - Name your budget (e.g., My-Budget-Alert).
    - Select projects to apply the budget to.
    - Set budget amount (fixed or based on last month’s spend).
    - Define thresholds (e.g., 25%, 50%, 90%, 100%).
    - Choose actual vs. forecasted spend.
    - Optionally, connect to Pub/Sub for automation (e.g., trigger workflows when thresholds are hit).
- Alerts are sent via email and can be monitored in the console.

## Exporting Billing Data
- Go to Billing → Billing Export.
- Two export options:
    - BigQuery: For advanced querying and dashboarding.
    - Cloud Storage: Export as CSV or JSON to a bucket.
- You must:
    - Create a BigQuery dataset or Cloud Storage bucket.
    - Define prefixes and destination.
    - Enable export and save settings.
- Transaction History
    - View detailed usage and charges by service (e.g., Compute Engine, disks).
    - Credits are shown as offsets to charges.