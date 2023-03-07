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
| Contributor | Allow all actions, except write or delete role assignment | * | - `Microsoft.Authorization/*/Delete`
- `Microsoft.Authorization/*/Write`
- `Microsoft.Authorization/elevateAccess/Action` | 
| Reader | Allow all read actions | /*/read | n/a |

