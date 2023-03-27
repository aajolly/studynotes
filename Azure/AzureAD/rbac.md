## Azure RBAC Concepts
| Concept | Description | Examples | AWS Reference |
| :-------: | :-------: | :-------: | :-------: |
| Security principal | An object that represents something that requests access to resources. | User, group, service principal, managed identity | Principal |
| Role definition | A set of permissions that lists the allowed operations. Azure RBAC comes with built-in role definitions, but you can also create your own custom role definitions. | Some built-in role definitions: Reader, Contributor, Owner, User Access Administrator | Actions in IAM Policy |
| Scope | The boundary for the requested level of access, or "how much" access is granted. | Root, management group, subscription, resource group, resource | Resource in IAM Policy |
| Assignment | An assignment attaches a role definition to a security principal at a particular scope. Users can grant the access described in a role definition by creating (attaching) an assignment for the role. | - Assign the User Access Administrator role to an admin group scoped to a management group
- Assign the Contributor role to a user scoped to a subscription | |

### Role Definition
A role definition consists of sets of permissions that are defined in a JSON file. Each permission set has a name, such as Actions or NotActions that describes the purpose of the permissions. Some examples of permission sets include:

- Actions permissions identify what actions are allowed.
- NotActions permissions specify what actions aren't allowed.
- DataActions permissions indicate how data can be changed or used.
- AssignableScopes permissions list the scopes where a role definition can be assigned.

The following table shows how the Actions or NotActions permissions are used in the definitions for three built-in roles: Owner, Contributor, and Reader.

| Role name | Description | Actions permissions | NotActions permissions |
| :-------: | :-------: | :-------: | :-------: |
| Owner | Allow all actions | * | n/a |
| Contributor | Allow all actions, except write or delete role assignment | * | `Microsoft.Authorization/*/Delete` `Microsoft.Authorization/*/Write` `Microsoft.Authorization/elevateAccess/Action` | 
| Reader | Allow all read actions | `/*/read` | n/a |

### Users in Azure Active Directory
In Azure AD, all user accounts are granted a set of default permissions. A user's account access consists of the type of user, their role assignments, and their ownership of individual objects.

#### Permissions and roles
**Permissions** allow you to control what access is granted to users or groups. This is done through **roles**. Many default roles exist with Azure AD, for example the Administrator Role that grants users access to create/edit users, assign administrative roles to others, reset user passwords, manage user licenses, and more.

#### Member Users
It is a native member of the Azure AD organization that has a set of default permissions like being able to manage their profile information. A member user is meant for users who are considered internal to an organization and are members of the Azure AD organization.

#### Guest Users
- Guest users have restricted Azure AD organization permissions.  
- Guest users sign in with their own work, school, or social identities. 
- By default, Azure AD member users can invite guest users. This default can be disabled by someone who has the User Administrator role.
- Your organization might need to work with an external partner. To collaborate with your organization, these partners often need to have a certain level of access to specific resources. For this sort of situation, it's a good idea to use guest user accounts.

##### Azure AD User Creation/Deletion
Use the below command to create a users using Azure CLI
```
az ad user create
```
Users can be created in bulk as well using a csv file.

Delete the user using the below command
```
az ad user delete
```

When you delete a user, the account remains in a suspended state for 30 days. During that 30-day window, the user account can be restored.


