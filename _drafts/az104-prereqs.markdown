---
date: 2020-11-27
title: "AZ-104 Prerequisites for Azure administrators"
---
# AZ-104 Prerequisites for Azure administrators

## Apply and monitor infrastructure standards with Azure Policy

Azure Policy is an Azure service you use to create, assign and, manage policies (rules and effects on resources).

Policies are evaluated on resource create and update actions.

Azure Policy vs RBAC:

- RBAC enforces user actions at different scopes
- Policy enforces resource properties during deployment. Policy is default-allow-and-explicit-deny system.

Steps: Create a policy (definition), assign a scope (assignment), view the results (compliance).

Many predefined policies exist (in the portal and Github) to use and customize.

policies have *replacement tokens* -- placeholders for parameter values (or defaults) given when the policy is defined for a scope.

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

The following video gives a good overview of the piecesof the governance story:

<iframe width="560" height="315" src="https://www.youtube.com/embed/LW8tGt9GvGg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Service Compliance through:

- Microsoft Privacy Statement - how Microsoft handles private data
- Microsoft Trust Center - a resource site for security, privacy, compliance, and transparency
- Service Trust Portal - hosts audit reports and compliance manager
- Compliance Manager - a workflow-based risk assessment dashboard

Azure Monitor - collects, analyzes, acts on telemetry data from your cloud (and optionally on-prem) environments. Telemtry has tiers: application, guest OS, resource, subscription, and tenant (Azure AD) levels.

Additional diagnostic options are available e.g. for virtual machine data.

Azure monitor includes:

- Application Insights
- Container monitoring
- VM monitoring

Azure monitor can respond to alerts and can autoscale your app.

Monitoring data can be pushed to views, dashboard, and Power BI.

Azure Service Health - suite for providing general Azure health information:

- Azure status - global view of health of Azure services
- Service health - Customizable to your services in your regions
- Resource health - Health and metrics of your specific resources

## Introduction to Azure virtual machines

Considering a VM:

- Start with the network - no default external access, no overlapping IP ranges, segregate the network (subnets), by default subnets in same VNet CAN communicate. Can be controlled with network security groups (NSGs)
- Name the VM - use naming convention e.g env, location, instance, product/service, role
- Decide the location for the VM
- Determine the size of the VM - can be resized, but can lose IP address
- Understanding the pricing model - pay as you go vs reserved
- Storage for the VM - unmanaged vs managed (newer)
- Select an operating system

Azure has different pre-configured workload-type VMs to choose from: memory-optimize, storage-optimized, compute-optimized

Azure Automation - automate frequent, time-consuming, and error-prone management tasks. Can be scheduled or triggered.

Availability set - a logical group of VMs to ensure not host upgrades don't happen to all VMs at the same time

Fault domain - a logical group of hardware that shares common power and network switch. The first two VMs in an availability set are deployed to different fault domains

Update domain - a logical group of hardware that can undergo maintainence/reboot at the same time. Availability groups are placed into update domains

Azure Site Recovery - replicates workloads from a primary site to a secondary location

Azure Backup supports a wide range of scenarios - file/folder, volume snapshots, database, cloud & on-prem

Azure backup works with other services; storage mgmt options, unlimited data transfer, encryption, scaling, long-term retention. Azure backup has several different components to install on each client, varying by type

## Fundamentals of computer networking

## Fundamentals of network security

## Control Azure services with the CLI

## Automate Azure tasks using scripts with PowerShell

## Secure your identities by using Azure Active Directory

## Introduction to Docker containers

## Choose a data storage approach in Azure
