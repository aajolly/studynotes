# Admin Policy
This is fairly long but gives a Vault administrator capabilities for most paths that they will need assuming they enable secrets engines and auth methods on the standard paths.
<details>
  <summary>Sample Admin Policy</summary>
  
  ```
# Manage auth methods broadly across Vault
path "auth/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Create, update, and delete auth methods
path "sys/auth/*" {
  capabilities = ["create", "update", "delete", "sudo"]
}

# List auth methods
path "sys/auth" {
  capabilities = ["read"]
}

# Create and manage ACL policies
path "sys/policies/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# To list policies - Step 3
path "sys/policies/" {
  capabilities = ["list"]
}

# List, create, update, and delete key/value secrets mounted under secret/
path "secret/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# List secret/
path "secret/" {
  capabilities = ["list"]
}

# Prevent admin users from reading user secrets
# But allow them to create, update, delete, and list them
path "secret/users/*" {
  capabilities = ["create", "update", "delete", "list"]
}

# List, create, update, and delete key/value secrets mounted under kv/
path "kv/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# List kv/
path "kv/" {
  capabilities = ["list"]
}

# Prevent admin users from reading user secrets
# But allow them to create, update, delete, and list them
# Creating and updating are explicitly included here
# Deleting and listing are implied by capabilities given on kv/* which includes kv/delete/users/* and kv/metadata/users/* paths
path "kv/data/users/*" {
  capabilities = ["create", "update"]
}

# Active Directory secrets engine
path "ad/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Alicloud secrets engine
path "alicloud/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# AWS secrets engine
path "aws/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Azure secrets engine
path "azure/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Google Cloud secrets engine
path "gcp/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Google Cloud KMS secrets engine
path "gcpkms/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Consul secrets engine
path "consul/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Cubbyhole secrets engine
path "cubbyhole/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Database secrets engine
path "database/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Identity secrets engine
path "identity/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# PKI secrets engine
path "nomad/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Nomad secrets engine
path "pki/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# RabbitMQ secrets engine
path "rabbitmq/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# SSH secrets engine
path "ssh/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# TOTP secrets engine
path "totp/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Transit secrets engine
path "transit/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Create and manage secrets engines broadly across Vault.
path "sys/mounts/*"
{
  capabilities = ["create", "read", "update", "delete", "list"]
}

# List sys/mounts/
path "sys/mounts" {
  capabilities = ["read"]
}

# Check token capabilities
path "sys/capabilities" {
  capabilities = ["create", "update"]
}

# Check token accessor capabilities
path "sys/capabilities-accessor" {
  capabilities = ["create", "update"]
}

# Check token's own capabilities
path "sys/capabilities-self" {
  capabilities = ["create", "update"]
}

# Audit hash
path "sys/audit-hash" {
  capabilities = ["create", "update"]
}

# Health checks
path "sys/health" {
  capabilities = ["read"]
}

# Host info
path "sys/host-info" {
  capabilities = ["read"]
}

# Key Status
path "sys/key-status" {
  capabilities = ["read"]
}

# Leader
path "sys/leader" {
  capabilities = ["read"]
}

# Plugins catalog
path "sys/plugins/catalog/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# List sys/plugins/catalog
path "sys/plugins/catalog" {
  capabilities = ["read"]
}

# Read system configuration state
path "sys/config/state/sanitized" {
  capabilities = ["read"]
}

# Use system tools
path "sys/tools/*" {
  capabilities = ["create", "update"]
}

# Generate OpenAPI docs
path "sys/internal/specs/openapi" {
  capabilities = ["read"]
}

# Lookup leases
path "sys/leases/lookup" {
  capabilities = ["create", "update"]
}

# Renew leases
path "sys/leases/renew" {
  capabilities = ["create", "update"]
}

# Revoke leases
path "sys/leases/revoke" {
  capabilities = ["create", "update"]
}

# Tidy leases
path "sys/leases/tidy" {
  capabilities = ["create", "update"]
}

# Telemetry
path "sys/metrics" {
  capabilities = ["read"]
}

# Seal Vault
path "sys/seal" {
  capabilities = ["create", "update", "sudo"]
}

# Unseal Vault
path "sys/unseal" {
  capabilities = ["create", "update", "sudo"]
}

# Step Down
path "sys/step-down" {
  capabilities = ["create", "update", "sudo"]
}

# Wrapping
path "sys/wrapping/*" {
  capabilities = ["create", "update"]
}

## Enterprise Features

# Manage license
path "sys/license/status" {
  capabilities = ["create", "read", "update"]
}

# Use control groups
path "sys/control-group/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# MFA
path "sys/mfa/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# List MFA
path "sys/mfa/" {
  capabilities = ["list"]
}

# Namespaces
path "sys/namespaces/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# List sys/namespaces
path "sys/namespaces/" {
  capabilities = ["list"]
}

# Replication
path "sys/replication/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

# Seal Wrap
path "sys/sealwrap/rewrap" {
  capabilities = ["create", "read", "update"]
}

# KMIP secrets engine
path "kmip/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

```
</details>

## Register admin policy
```bash
vault policy write admin /root/config-files/policies/admin-policy.hcl
```

## Create an admin token
Next, create a Vault token role called admin that is assigned the admin policy and is configured so that tokens created from it are periodic and orphaned:
```bash
vault write auth/token/roles/admin allowed_policies=admin token_period=2h orphan=true
```
> **_Note_**: Note that a periodic token is automatically renewed by Vault at the end of each period (two hours in this case) and never expires.

We want the token created from the admin role to be an orphan token so that it will not be revoked when you revoke the initial root token below.

Next, please generate a periodic, orphaned token with display name admin-1 from the admin token role:
```bash
vault token create -display-name=admin-1 -role=admin
```
This should return something like this
```bash
Key                  Value
---                  -----
token                hvs.CAESIGxWWBpSrfyrvE3eTmQchxSmociTGe9jCFmRZS0BcB3PGiAKHGh2cy43MXNRTnpSeEhkelJIc3JFTFRWRUtMaFMQIA
token_accessor       QHI4fHIY7oAYjQLj6TeootOP
token_duration       2h
token_renewable      true
token_policies       ["admin" "default"]
identity_policies    []
policies             ["admin" "default"]
```
## Namespaces
Vault Enterprise Namespaces allow Vault to support multi-tenant deployments in which different teams are isolated from each other and can self-manage their own secrets.

In this challenge, you will create two namespaces called "Sales" and "Engineering", enable the Userpass authentication method and KVv2 secrets engine in them, and define Vault policies for them. These new namespaces will live underneath the "root" namespace

## LDAP Method
The LDAP auth method must know how to resolve which groups the user is a member of. The configuration for this can vary depending on your LDAP server and your directory schema.

There are two main strategies when resolving group membership:
1. The first is searching for the authenticated user object and following an attribute to groups it is a member of.
2. The second is to search for group objects of which the authenticated user is a member of.

Enable the first LDAP auth method instance you will configure for Unique Members at the path ldap-um:
```bash
vault auth enable -path=ldap-um ldap
```
Follow this up by creating the second LDAP auth method instance that will be used for looking up via MemberOf:
```bash
vault auth enable -path=ldap-mo ldap
```

Now that you have enabled the authentication methods you need to configure them to work with an LDAP server.

With LDAP and Active Directory there are any number of ways an organization can setup the structure. A best practice is to always have someone familiar with this to help provide connection information.

To configure Vault to talk to the LDAP server you will need to configure some information. Let's look at a sample configuration for our first method of Unique Members:
```bash
vault write auth/ldap-um/config \
    url="${LDAP_URL}" \
    binddn="${BIND_DN}" \
    bindpass="${BIND_PW}" \
    userdn="${USER_DN}" \
    userattr="${USER_ATTR}" \
    groupdn="${GROUP_DN}" \
    groupfilter="${UM_GROUP_FILTER}" \
    groupattr="${UM_GROUP_ATTR}" \
    insecure_tls=true
```
- `url` defines the network address of the LDAP server.
- `binddn` and `bindpass` are the two parameters that tell Vault what user it should use to query the LDAP server. This can be provided by the identity management team in an organization.
_Example_: binddn -> cn=read-only,dc=ourcorp,dc=com, bindpass -> devsecopsFTW
- `userdn` defines where in the tree to start an LDAP search.
_Example_: `userdn` -> ou=people,dc=ourcorp,dc=com
- `userattr` defines what the key will be to find the username. This can vary based on implementation.
_Example_: `userattr` -> cn
- `groupdn` defines another search scope against the group_dn.
_Example_: `groupdn` -> ou=um_group,dc=ourcorp,dc=com
- `groupfilter` establishes the search filter to return a list of unique memebers.
_Example_: `groupfilter` will be (&(objectClass=groupOfUniqueNames)(uniqueMember={{.UserDN}}))
- `groupattr` is the attribute to be returned from the search.
_Example_: `groupattr` -> cn

Next, repeat the previous step but change the `groupfilter` and `groupattr` attributes to reflect searching for memberOf instead of uniqueMember.
```bash
vault write auth/ldap-mo/config \
   url="ldap://${LDAP_SERVER}" \
   binddn="cn=read-only,dc=ourcorp,dc=com" \
   bindpass="devsecopsFTW" \
   userdn="ou=people,dc=ourcorp,dc=com" \
   userattr="cn" \
   groupdn="ou=people,dc=ourcorp,dc=com" \
   groupfilter="(&(objectClass=person)(uid={{.Username}}))" \
   groupattr="memberOf" \
   insecure_tls=true
```
Now that we have configured both LDAP auth method instances we need to create policies that we will use to bind users when they authenticate to Vault with access policies that will give them access to specific paths in the KV secrets engine.

Vault policies control access to secrets. In order to give authenticated users access to secrets a policy must be bound to either that user or a group of users they belong to.

We are going to define two types of polices. One will be a static policy that will be applied to a group while the other will be a dynamic policy that will be applied to all users. This dynamic policy allows for dynamic paths to be created based on an identity alias.

Consider the policy below:
<details>
  <summary>Sample Admin Policy</summary>
  
  ```bash
    # Allow full access to the current version of the kv-blog
    path "kv-blog/data/{{identity.entity.aliases.${UM_ACCESS}.name}}/*"
    {
    capabilities = ["create", "read", "update", "delete", "list"]
    }
    path "kv-blog/data/{{identity.entity.aliases.${UM_ACCESS}.name}}"
    {
    capabilities = ["create", "read", "update", "delete", "list"]
    }
    # Allow deletion of any kv-blog version
    path "kv-blog/delete/{{identity.entity.aliases.${UM_ACCESS}.name}}/*"
    {
    capabilities = ["update"]
    }
    path "kv-blog/delete/{{identity.entity.aliases.${UM_ACCESS}.name}}"
    {
    capabilities = ["update"]
    }
    # Allow un-deletion of any kv-blog version
    path "kv-blog/undelete/{{identity.entity.aliases.${UM_ACCESS}.name}}/*"
    {
    capabilities = ["update"]
    }
    path "kv-blog/undelete/{{identity.entity.aliases.${UM_ACCESS}.name}}"
    {
    capabilities = ["update"]
    }
    # Allow destroy of any kv-blog version
    path "kv-blog/destroy/{{identity.entity.aliases.${UM_ACCESS}.name}}/*"
    {
    capabilities = ["update"]
    }
    path "kv-blog/destroy/{{identity.entity.aliases.${UM_ACCESS}.name}}"
    {
    capabilities = ["update"]
    }
    # Allow list and view of metadata and to delete all versions and metadata for a key
    path "kv-blog/metadata/{{identity.entity.aliases.${UM_ACCESS}.name}}/*"
    {
    capabilities = ["list", "read", "delete"]
    }
    path "kv-blog/metadata/{{identity.entity.aliases.${UM_ACCESS}.name}}"
    {
    capabilities = ["list", "read", "delete"]
    }
    # Allow full access to the current version of the kv-blog
    path "kv-blog/data/{{identity.entity.aliases.${MO_ACCESS}.name}}/*"
    {
    capabilities = ["create", "read", "update", "delete", "list"]
    }
    path "kv-blog/data/{{identity.entity.aliases.${MO_ACCESS}.name}}"
    {
    capabilities = ["create", "read", "update", "delete", "list"]
    }
    # Allow deletion of any kv-blog version
    path "kv-blog/delete/{{identity.entity.aliases.${MO_ACCESS}.name}}/*"
    {
    capabilities = ["update"]
    }
    path "kv-blog/delete/{{identity.entity.aliases.${MO_ACCESS}.name}}"
    {
    capabilities = ["update"]
    }
    # Allow un-deletion of any kv-blog version
    path "kv-blog/undelete/{{identity.entity.aliases.${MO_ACCESS}.name}}/*"
    {
    capabilities = ["update"]
    }
    path "kv-blog/undelete/{{identity.entity.aliases.${MO_ACCESS}.name}}"
    {
    capabilities = ["update"]
    }
    # Allow destroy of any kv-blog version
    path "kv-blog/destroy/{{identity.entity.aliases.${MO_ACCESS}.name}}/*"
    {
    capabilities = ["update"]
    }
    path "kv-blog/destroy/{{identity.entity.aliases.${MO_ACCESS}.name}}"
    {
    capabilities = ["update"]
    }
    # Allow list and view of metadata and to delete all versions and metadata for a key
    path "kv-blog/metadata/{{identity.entity.aliases.${MO_ACCESS}.name}}/*"
    {
    capabilities = ["list", "read", "delete"]
    }
    path "kv-blog/metadata/{{identity.entity.aliases.${MO_ACCESS}.name}}"
    {
    capabilities = ["list", "read", "delete"]
    }
  ```
</details>
Notice the `{{identity.entity.aliases.${UM_ACCESS}.name}}` entry in the paths. This tells Vault to insert the username from the auth method and to create a dynamic path based on that attribute and the accessor of the LDAP auth method.

Create Policy using the below command
```bash
vault policy write kv <policy_name>.hcl
```
Now that you have the authentication methods, policies and secret engine configured, it is time to link them together.
```bash
vault write auth/ldap-um/groups/it policies=<policy_name>
```

> **_Note_**: The group that you create in Vault must exist in LDAP or it will not be found and authenticated against. Remember the groupfilter attribute that you created? This group will be returned with all the userDNs. That is how Vault will match and authenticate a user and know what policy to assign to it.

> **_Note_**: You can also set a policy to a single user.

> **_Note_**: Policies are additive in nature; if you have a group policy and a user policy, the token will inherit both of them. The only time this is not true is if the capabilities have deny on a path; in that case, the deny capability overrides all other capabilities for that path.
