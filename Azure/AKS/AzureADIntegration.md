# Overview
This section helps secure the AKS cluster by integrating with Azure Active Directory to provide authentication. Kubernetes doesn't provide an identity management solution to tie Azure users to Kubernetes users. With Azure Active Directory (Azure AD), you can create roles or clusterRoles that allow access to Kubernetes resources, then bind those roles to users in Azure AD.

1. Create an Azure AD group
This group will be registered as an admin group on the cluster to grant cluster admin permissions. You can use an existing Azure AD group or create a new one. Make sure to record the object ID of your Azure AD group.
```bash
az ad group create `
 --display-name aajollyAKSAdminGroup `
 --mail-nickname aajollyAKSAdminGroup
 ```

 2. Enable Azure AD integration on existing cluster
 ```bash
 az aks update `
  -g aajolly-aks-rg `
  -n aajollyAKSCluster `
  --enable-aad `
  --aad-admin-group-object-ids <object ID of group created in Step1> `
  --aad-tenant-id <tenantID>
  ```

  ***Note:** TenantID can be found using the command `az account tenant list`