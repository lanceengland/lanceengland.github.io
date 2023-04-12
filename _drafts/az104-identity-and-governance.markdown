---
date: 2020-11-27 -- todo
tags: integration automation
title: "AZ-104 Identity and Governance"
---

# AZ-104 Identity and Governance

> AZ-104 Blog Series
>
> - [Learning plan](/2020/12/az104-learning-plan)
> - [Prerequisites](/2020/12/az104-prereqs)
> - **Identities and governance**
> - Storage
> - Compute
> - Networking
> - Back up

## Identity

Identity and access management (IAM) is implemented with Azure Active Directory (Azure AD),
a multi-tenant cloud-based directory service. An organization is assigned a reserved instance
of Azure AD, called a tenant.

> A tenant represents an organization.

### Active Directory vs Azure AD

| | Active Directory           | Azure AD                   |
|-|----------------------------|----------------------------|
|Scoping | Organization units (OU)    | Administrative units       |
| Security | Group Policy Objects (GPO) | RBAC                       |
| Protocols | Kerberos, LDAP, NTLM       | SAML, WS-Federation, OAuth |
| Structure | Hierarchical               | Flat                       |
| Location | On-prem                    | Multi-tenant cloud service |

### Azure AD Connect

Azure AD Connect can be used to sync identies to the cloud. Requirements to set up:

- todo

### Azure AD licenses

| Free                                                               | Premium P1                                                                                   | Premium P2                                            |
|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------|-------------------------------------------------------|
| On-premise sync, self-service password change for cloud users only | Adds dynamic groups, cloud write-back, conditional access policies (like 2FA), and self-service password change for on-premise users | Adds Identity Protection and Privledged Identity mgmt |

#### Users

| Cloud               | Hybrid                                 | Guest                                         |
|---------------------|----------------------------------------|-----------------------------------------------|
| Created in Azure AD | Directory-synchronized from on-prem AD | Azure AD B2B collaboration through invitation |

Adding users:

- Trough portal, PowerShell, CLI
- Invite (through portal or PowerShell New-AzureADMSInvitation)
- External Active Directory synced through Azure Connect
- Bulk operations (in portal): Bulk Create, Bulk Invite, Bulk Delete

Noe: Bulk property EDITS requires PowerShell + Azure AD module.

> The AzureAD PowerShell module is seperate from the Az module.

#### Groups

Groups can have owners assigned for non-admins to manage.

Membership can be assigned (manual) or dynamic (exam alert. Write an expression based on AD object properties).

Groups can be assigned roles and licenses that are inherited/transferred to members.

Types:

- Security (similar to local AD)
- Microsoft 365 group (if applicable)

Users and groups can be added to administrative units to reduce scope of
assigning administrative permissions.

Group membership can be static (assigned) or dynamic (rule-based e.g. Dept = 'HR')

#### Devices

A computer/device that is CAPABLE of joining Azure AD tenant and being managed via mobile device management (MDM) tools.

| Registered                                    | Joined                                                                  |
|-----------------------------------------------|-------------------------------------------------------------------------|
| Personal device (Win 10, iOS, Android, macOS) | Organization-owned devices, Azure AD sign-in. Full control via policies |

#### Multi-factor Authentication (MFA)

MFA is implemented with conditional access policy.

It can be assigned to users, groups, or directory roles.

It can be assigned to cloud apps e.g. Office 365.

Under Grant -> ENable multifactor authentication.

Users will have 14 days to register for MFA using the Microsoft Authenticator.

MFA can be enabled on a per-user basis.

Guest users can be assigned MFA.

## Governance

> Azure Governance = Management Groups/Subscriptions + RBAC + Policies/Blueprints + Locks/Tags

Azure AD Built-in Roles - over 70 built-in built-in roles for delegating access to manage Azure AD, Microsoft 365, Skype, SharePoint, Power BI, etc. An almost overwhelming list of fine-grained roles.

Classic subscription administrator roles:

- Account Administrator - 1 per account
- Service Administrator - 1 per subscription
- Co-Administrator - 200 per subscription

Role-Based Access Control (RBAC):

- Scopes - Management groups, subscriptions, resource groups, or resources
- Properties - Actions/Allow, NotActions/Deny, DataActions/data plane allow, NotDataActions/data plane deny
- Pattern - Owner (full-control, classic Service and co-admin are in this), Contributor (full-control except granting permissions), Reader (read only), User Access Admin (granting permissions). Each resource also has implementation of patern e.g. VM Contributor
- Custom roles - Existing roles defintions can be exported and edited to create custom roles
- Assignment - User, group, service principal, managed principal
- Permissions are inherited from parent scope.

![How roles are related](../assets/img/az104/rbac-admin-roles.png)

Image Credit: [docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles)

### Subscriptions and Management groups

A subscription is a billing container and can be organized with management groups. Management groups can contain subscriptions, other management groups, and resource groups (which contain resources).

Good practice: Multiple subscriptions for different environments e.g. Dev, Prod, etc.

A subscription CAN be transferred between different tenants, but can only belong to one tenant at a time. A tenant CAN have multiple subscriptions.

Subscriptions are linked to accounts (Billing Account or Global Admin). An account can have amultiple subscriptions, but a subscription can only belong to one account.

A single AD has 10,000 management group limit

A management group tree has 6-level limit (not including root and subscription)

### Moving resources

Resources CAN be moved between:

1. Resource groups
2. Subscriptions
3. Regions ([https://docs.microsoft.com/en-us/azure/resource-mover/overview](Resource mover tool)

Resources that can move across REGIONS with Resource Mover:

- Azure VMs and associated disks (including encrypted)
- NICs
- Availability sets
- Azure virtual networks
- Public IP addresses
- Network security groups (NSGs)
- Internal and public load balancers
- Azure SQL databases and elastic pools

Notes:

Moving between resource groups and subscriptions is a meta-data operation.

Moving between REGIONS requires the Azure Resource Mover tool (though it can also perform RG and subscription moves).

Source and target resource group are locked until the move is complete.

> Moving a resource to a different group or subscription does NOT change the location of the resource.

Moving changes the resource ID because both subscription and reource group are part of the resource ID.

The resource must support the move operation. Dependent resources must also be moved.

Source and destination subscription must be active and in the same tenant.

Destination subscription must be registered for the resource type.

Subscriptions can be moved between management groups.

### Cost Management

The Cost Management blade in the portal contains:

- Cost analysis - Shows trends and forecasts
- Cost Alerts - Define alerts when a threshold is met
- Budgets - Apply budgets to enforce thresholds to limit spend
- Recommendations - Suggested cost-savings based on usage

### Tags

Tags can be applied to almost everything. Names are case INSENSITIVE and values are case SENSITIVE. Many use cases. Best practice: Use for billing/cost center. Can use policies to enforce during resource creation, and flag existing non-compliant resources.

Name max length 512, value max length 256. Exception: storage accounts tag name max length 128. Why? No idea. Also, a resoruce can have a max of 50 tags.

PowerShell can be used to bulk-apply tags. Be careful not to REPLCE the tag collection; rather, you would want to ADD to the existing tag collection.

> Tags are NOT inherited from parents e.g. resource groups.

Resource tags are name/value pairs for organization. This helps for relating items from different resource groups. Spending forecasts and reports can be filtered by tags, for example.

### Locks

Locks are used to prevent accidents. Two types of locks:

1. CanNotDelete
2. ReadOnly - do allow moving resources between subscription, resource group, region.

> Locks assigned to resource groups ARE inherited by its child resources.

### Azure Policy

A way to define rules for and effects to be applied at different scope.
Azure has many built-in policies.

Policies are evaluated during resource create and update actions.

Policies will prevent new resources that do not comply, but only FLAGS existing non-compliant resources (it will not remove them).

Best practice: Organize policies with initiatives; a set of policy definitions. The initiative would then be defined at a scope.

Effects:

- Deny - create/update fails
- Disabled - policy is ignored (good for testing)
- Append - Adds to the resource e.g. tags
- Audit; AuditIfNotExists - Warning to activity log, but continues
- DeployIfNotExists - Execute a template deployment
