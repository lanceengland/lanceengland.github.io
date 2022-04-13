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

This blog post is part of a series in prepping for [Exam AZ-104: Microsoft Azure Administrator](https://docs.microsoft.com/en-us/learn/certifications/exams/az-104). The learning objectives for Identities and Governance are below.

Manage Azure Active Directory (Azure AD) objects

- create users and groups
- create administrative units
- manage user and group properties
- manage device settings
- perform bulk user updates
- manage guest accounts
- configure Azure AD join
- configure self-service password reset

Manage role-based access control (RBAC)

- create a custom role
- provide access to Azure resources by assigning roles at different scopes
- interpret access assignments

Manage subscriptions and governance

- configure Azure policies
- configure resource locks
- apply and manage tags on resources
- manage resource groups
- manage subscriptions
- manage costs
- configure management groups

## Manage Azure Active Directory (Azure AD) objects

### Azure AD

The path to using Azure starts with Azure AD. Azure Active Directory (Azure AD) is Microsoft’s multi-tenant cloud-based directory and identity management service. When an organization signs up to use a Microsoft service like Azure or Microsoft 365, it is assigned a reserved service instance of Azure AD, called a tenant.

> A tenant represents an organization.

A principal is an unknown entity to be authenticated to an identity. An identity can represent a person, server, or service. Azure AD ia a global Identity and Access Management (IAM) service. A tenant (or directory) is a single instance of Azure AD representing a single organization. A Tenant is automatically created when your organization signs up for a Microsoft cloud service subscription.

| Active Directory           | Azure AD                   |
|----------------------------|----------------------------|
| Organization units (OU)    | Administrative units       |
| Group Policy Objects (GPO) | RBAC                       |
| Kerberos, LDAP, NTLM       | SAML, WS-Federation, OAuth |
| Hierarchical               | Flat                       |
| On-prem                    | Multi-tenant cloud service |

You can combine the two through Azure AD Connect for hybrid solutions.

Three flavors 1) Free, 2) Premium P1, 3) Premium P2

Free allows on-premise sync, self-service password change for cloud users only

P1 adds dynamic groups, cloud write-back, and self-service password change for on-premise users.

P2 adds Identity Protection and Privledged Identity mgmt

Azure AD default directory has a Tenant ID

P1 and P2 offer a one-time 30-day trial

Three Azure AD roles:

1. Global Admin
2. User Admin
3. Billing Admin

### Identity

Tenant - an instance of an Azure AD directory

Three principal types of user accounts in Azure AD

1. Cloud identity
  a. 'Local' Azure AD
  b. External Azure AD
2. Hybrid identity - Directory-syncronized from on-prem AD
3. Guest identity - Azure AD B2B collaboration through invitation
  a. User clicks redemption link
  b. Direct federation/Google federation/Microsoft (MSA) -> OK
  c. Otherwise, one-time passcode

Note: The AzureAD PowerShell module is seperate from the Az module.

1. Install-Module AzureAD
2. Connect-AzureAD
3. New-Object -Type Microsoft.Open.AzureAD.Model.PasswordProfile
4. Set properties
5. New-AzureADUser -PassWordProfile $PasswordProfile etc.

CLI method: az ad user create --display-name "Bob" --password "" etc.

### Bulk-creation tool

Under Users->All Users, a UI for bulk create, invite, delete, download. A CSV can be imported, etc.

Bulk property EDITS requires PowerShell + Azure AD module.

### Azure AD Groups

Types:

- Security (similar to local AD)
- Microsoft 365 group (if applicable)

Groups can have owners assigned for non-admins to manage.

Membership can be assigned (manual) or dynamic (exam alert. Write an expression based on AD object properties).

Groups can be assigned roles and licenses that are inherited/transferred to members.

Azure AD has administrative units, analogous to organizational units (OU) in AD. Can delegate administrative permissions to an administrative unit (reset password, manage membership)!

PowerShell experience similar to AzureAD user:

1. Install-Module AzureAD
2. Connect-AzureAD
3. New-AzureADGroup -Description "Marketing" -DisplayName etc
4. Add-AzureADGroupMember -ObjectId "guid" -RefObjectId "guid"

ObjectId = Group guid, RefObjectId = User guid

CLI: az ad group create, az ad group member check --group Sales --member-id xxx-xxx, az ad group member add --group Sales --member-id xxx-xxx

Classic subscription roles vs RBAC vs Azure AD roles

![Roles overview](https://docs.microsoft.com/en-us/azure/role-based-access-control/media/rbac-and-directory-admin-roles/rbac-admin-roles.png)

Classic is at subscription level. RBAC adds > 70 built-in resource-scoped roles. Azure AD roles are for Azure AD resources. Read more: [Classic subscription administrator roles, Azure roles, and Azure AD roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles)

### Devices

A computer/device that is CAPABLE of joining Azure AD tenant and being managed via mobile device management (MDM) tools.

Registered vs Joined:

- Registered - a personal device w/ MSA or local acct sign-in (supports Win 10, iOS, Android, macOS).
- Joined - an organization-owned devices, Azure AD sign-in. Full control via policies.

### Azure AD Join

Azure AD join allows you to join devices directly to Azure AD without adding them to on-prem AD. Azure AD join enables you to transition towards a cloud-first model.

## Manage role-based access control (RBAC)

Over 70 built-in roles. Four are fundamental:

1. Owner - Serbice and co-admin from classic are added to this
2. Contributor - Can't grant access, but manage resources
3. Reader - View
4. User Access Admin - Manage user access

Roles can be assigned to user, group, service principal (created for applications), or managed principals (identity automatically managed by Azure).

Scope: Roles can to a resource, or a resoucr group, a subscription. Permissions are inherited from parent scope.

Roles have four important properties:

1. Actions - Allow
2. NotActions - Deny
3. DataActions - Blob-specific allow
4. NotDataActions - Blob-specific deny

Custom roles have the above properties + assignable scope. Custom roles are defined in JSON.

## Manage subscriptions and governance

### Subscriptions

At the of the organizational structure is your **Azure AD tenant**, an instance of Azure AD that represents an organization. Right below this is the root management group, which cannot be moved or deleted. The entire management hierarchy is organized under the root management group.

A **subscription** is a billing container. Subsriptions can be organized with **management groups**. Management groups can contain subscriptions, other management groups, and resource groups (which contain resources). The image below is a good summary.

![Source: https://docs.microsoft.com/en-us/azure/governance/management-groups/media/tree.png](/assets/img/az104/mgmt_group_tree.png)

Important!

- You CAN move resources between subscriptions (in general, a few have conditions)
- A subscription CAN be transferred between different tenants, but can only belong to one tenant at a time.
- A tenant CAN have multiple subscriptions

Management groups let you group subscriptions together. You can also create a mgmt group hierarchy. The Root mgmt group can have child groups and subscriptions.

Subscriptions are linked to accounts (Billing Account or Global Admin). An account associate with multiple subscriptions (children), but a subscription can only be associated with one account (parent).

Good practice: Multiple subscriptions for different environments e.g. Dev, Prod, etc.

### Security Role Types

Three different types of [security roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles):

1. Classic roles
2. Azure AD
3. Azure RBAC

#### Classic roles

- Account Administrator - Only ONE per Azure account. The billing owner, and NO portal access
- Service Administrator - Only ONE per Azure subscription. Can cancel subscription and assign co-administrators ergo portal access.
- Co-Administrator - Up to 200 per Azure subscription. Can add co-administrators but cannot change service administrators.

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

Locks are used to prevent accidents. Two types of locks: 1) CanNotDelete and 2) ReadOnly

> Locks assigned to resource groups ARE inherited by its child resources.

#### Moving resources

Resources CAN be moved between:

1. Resource groups
2. Subscriptions
3. Regions

Moving between resource groups and subscriptions is a meta-data operation and is handled by the Azure Resource Manager, the underlying service that handles request from the portal, Azure CLI, PowerShell, REST, and SDK. Moving between REGIONS requires the Azure Resource Mover tool. The Azure Resource MOver is part of the portal to assist in all THREE move scenarios. Note: You can only do one move type at a time e.g you can't move regions and subscriptions at the same time.

Reminder: The location hierarchy in Azure is Georgraphies -> Regions -> Availability Zones -> Data Centers.

> The source and target resource groups are locked until the move is complete.

Many resources are unsupported, while others have special conditions, and others are fully-supported. The full list of [move operation support for resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/move-support-resources). Virtual machines have a dedicated [move guidance for VMs](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/move-limitations/virtual-machines-move-limitations) page for supported/unsupported scenarios.

Some supported resources for moving regions:

- VMs and disks (including encrypted)
- Availability sets
- Virtual networks
- Public IPs
- Network securty groups (NSG)
- Load balancers (internal and public)
- Azure SQL databases and elastic pools

Moving a resource requires moving dependent resources e.g. a VM would also need to move public IP, the drive, the NIC. a VPN gateway MUST move the IP address too

If a resource group has a ReadOnly lock you CAN add/move resources to it. The resource group PROPERTIES are read-only, and it's resources inherit the ReadOnly lock (I think).

### Governance

> Azure Governance = Management Groups/Subscriptions + RBAC + Policies/Blueprints + Locks/Tags

Policies are applied to a scope (and child scopes). Global policies (and/or role-based access control) would be applied at the root management group. The AD Global admin needs to elevate to User Access Administrator role. New subscriptions default to root management group, and can see it, but don't have access to modify. Best practice: Be very careful with policies and especially RBAC at root management, because it would apply to EVERYTHING in that tenant.

### Azure Policy

Azure Policy is an Azure service you use to create, assign and, manage policies (rules and effects on resources).

Policies are evaluated during resource create and update actions.

Azure Policy is a service to create, assign, and manage policies. Policies can enforce rules and show non-compliance. Azure has many built-in policies.

Policies will prevent new resources that do not comply, but only FLAGS existing non-compliant resources (it will not remove them).

Policies can be applied to subsctions and management groups.

- Policy defintion - Defines the evaluation criteria, and the action for non-compliance (Audit or Deny)
- Policy assignment - The scope of assignment: Management group, subscription, resource group, resource
- Initiative definition - A collection of policies to achieve a singular goal e.g. VM standards

Azure Policy vs RBAC:

- RBAC enforces user actions at different scopes
- Policy enforces resource properties during deployment. Policy is default-allow-and-explicit-deny system.

Steps: Create a policy (definition), assign a scope (assignment), view the results (compliance).

Many predefined policies exist (in the portal and Github) to use and customize.

policies have *replacement tokens* - placeholders for parameter values (or defaults) given when the policy is defined for a scope.

A policy has ONE of the following effects when evaluated:

- Deny - create/update fails
- Disabled - policy is ignored (good for testing)
- Append - Adds to the resource e.g. tags
- Audit; AuditIfNotExists - Warning to activity log, but continues
- DeployIfNotExists - Execute a template deployment

Best practice: Organize policies with initiatives; a set of policy definitions. The initiative would then be defined at a scope.

Azure Management Groups - containers for managing access, policies, and compliance across *multiple* Azure subscriptions. Subscriptions inherit from parent management group.

On creating the first management group, a root management group is created in the Azure Active Directory (Azure AD) organization.

- Any AD user can create a management group (and assigned owner)
- A single AD has 10,000 management group limit
- A management group tree has 6-level limit (not including root and subscription)
- Each group can have many children
- New subscriptions are automatically added to the root management group

Azure Blueprints - a collection of ARM, RBAC, and policies for repeated, consistent deployment. Can be assigned to management group or subscription level.

Blueprints vs ARM template - ARM templates are 'external' to Azure; Blueprints are a package (including ARM templates) that maintain the connection between what should be deployed and what was deployed. Both are used together.

Blueprints vs Policy - Blueprints defone what should be deployed; policies allow/deny deployment and change on new and existing resources.

[Azure Blueprints Deep Dive video @Build](https://www.youtube.com/watch?v=d6c1nfoySLI&feature=youtu.be&t=2858)

[Azure Blueprints as code repo](https://github.com/Azure/azure-blueprints)

This video gives a good overview of the pieces of the [Azure governance story](https://youtu.be/LW8tGt9GvGg)
