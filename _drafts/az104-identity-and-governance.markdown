---
date: 2020-11-27
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

intro paragraph

## Summary

## Manage Azure AD objects

<!--
Create users and groups
Manage user and group properties
Manage device settings
Perform bulk user updates
Manage guest accounts
Configure Azure AD Join
Configure self-service password reset
NOT: Azure AD Connect; PIM
-->

### Azure AD

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

<!--
Create a custom role
Provide access to Azure resources by assigning roles subscriptions, resource groups, and resources (VM, disk, etc.)
Interpret access assignments
Manage multiple directories
-->

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

<!--
Configure Azure policies
Configure resource locks
Apply tags
Create and manage resource groups
Move resources
Remove RGs
Manage subscriptions
Configure Cost Management
Configure management groups
-->

### Subscriptions

At the of the organizational structure is your **Azure AD tenant**, an instance of Azure AD that represents an organization. Right below this is the root management group, which cannot be moved or deleted. The entire management hierarchy is organizaed under the root management group.

A **subscription** is a billing container. Subsriptions can be organized with **management groups**. Management groups can contain subscriptions, other management groups, and resource groups (which contain resources). The image below is a good summary.

![Source: https://docs.microsoft.com/en-us/azure/governance/management-groups/media/tree.png](/assets/img/az104/mgmt_group_tree.png)

Important!

- You CAN move resources between subscriptions (in general, a few have conditions)
- A subscription CAN be transferred between different tenants
- A tenant CAN have multiple subscriptions

Management groups let you group subscrptions together. You can also create a mgmt group hierarchy. The Root mgmt group can have child groups and subscriptions.

Subscriptions are linked to accounts. An account can have multiple subscriptions.

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

PowerShell can be used to bulk-apply tags. be CAREFUL not to replace the tag collection; rath, you would want to ADD to the tag collection.

> Tags are NOT inherited from parents e.g. resource groups.

Resource tags are name/value pairs for organization. This helps for relating items from different resource groups. Spending forecasts and reports can be filtered by tags, for example.

### Locks

Locks are used to prevent accidents. Two types of locks: 1) CanNotDelete and 2) ReadOnly

> Locks assigned to resource groups ARE inherited by its child resources.

#### Moving resources

Can move resources between resource groups or subscriptions. The source and target resource groups are locked until the move is complete.

Moving resources does NOT move the location.

Moving a VPN gateway MUST move the IP address too.

Moving a VM and NIC MUST move dependent resources too.

If a resource group has a ReadOnly lock you CAN add/move resources to it. The resource group PROPERTIES are read-only, and it's resources inherit the ReadOnly lock (I think).

### Governance

Important! Azure Management Groups + Azure Policy = Azure Governance

Policies are applied to a scope (and child scopes). Global policies (and/or role-based access control) would be applied at the root management group. The AD Global admin needs to elevate to User Access Administrator role. New subscriptions default to root management group, and can see it, but don't have access to modify. Best practice: Be very careful with policies and especially RBAC at root management, because it would apply to EVERYTHING in that tenant.

### Azure Policy

Azure Policy is an Azure service you use to create, assign and, manage policies (rules and effects on resources).

Policies are evaluated on resource create and update actions.

Azure Policy is a service to create, assign, and manage policies. Policies can enforce rules and show non-compliance. Azure has many built-in policies.

Policies will prevent new resources that do not comply, but only flags existing non-compliant resources (it will not remove them).

Policies can be applied to subsctions and management groups.

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
