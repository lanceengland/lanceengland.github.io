---
date: 2020-11-27
title: "AZ-104 Identity and Governance"
---
# AZ-104 Identity and Governance

## Identity

todo: resection with 1. identities, 2. governance

### Subscriptions

Management groups let you group subscrptions together. You can also create a mgmt group hierarchy. The Root mgmt group can have child groups and subscriptions.

Subscriptions are linked to accounts. An account can have multiple subscriptions.

Good practice: Multiple subscriptions for different environments e.g. Dev, Prod, etc.

Three different types of [security roles](https://docs.microsoft.com/en-us/azure/role-based-access-control/rbac-and-directory-admin-roles):

1. Classic roles
2. Azure AD
3. Azure RBAC

### Classic roles

- Account Administrator - Only ONE per Azure account. The billing owner, and NO portal access
- Service Administrator - Only ONE per Azure subscription. Can cancel subscription and assign co-administrators ergo portal access.
- Co-Administrator - Up to 200 per Azure subscription. Can add co-administrators but cannot change service administrators.

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

### Azure RBAC

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

### Azure AD Join

Azure AD join allows you to join devices directly to Azure AD without adding them to on-prem AD. Azure AD join enables you to transition towards a cloud-first model.

## Governance

Locks are used to prevent accidents. Two types of locks: 1) CanNotDelete and 2) ReadOnly

Locks assigned to resource groups are inherited by its child resources.

Azure Policy is a service to create, assign, and manage policies. Policies can enforce rules and show non-compliance. Azure has many built-in policies.

Policies will prevent new resources that do not comply, but only flags existing non-compliant resources (it will not remove them).

Policies can be applied to subsctions and management groups.

Resource tags are name/value pairs for organization. This helps for relating items from different resource groups. Spending forecasts and reports can be filtered by tags, for example.

### Moving resources

Can move resources between resource groups or subscriptions. The source and target resource groups are locked until the move is complete.

Moving resources does NOT move the location.

Moving a VPN gateway MUST move the IP address too.

Moving a VM and NIC MUST move dependent resources too.
