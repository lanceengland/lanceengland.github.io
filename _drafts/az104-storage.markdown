---
date: 2020-11-27
tags: integration automation
title: "AZ-104 Storage"
---
# AZ-104 Storage

> AZ-104 Blog Series
>
> - [Learning plan](/2020/12/az104-learning-plan)
> - [Prerequisites](/2020/12/az104-prereqs)
> - [Identities and governance](/2020/12/az104-identity-and-governance)
> - **Storage**
> - Compute
> - Networking
> - Back up

## Properties

URLs follow the pattern

https://[storage account name].file.core.windows.net/[file share name]

https://[storage account name].blob.core.windows.net/[container name]

https://[storage account name].queue.core.windows.net/[queue name]

https://[storage account name.table.core.windows.net/[table name]


## Types

## Resiliency options

## Performance options

## Access tiers and lifecycle management

## Object Replication

## RBAC

## Managed Disks

## File Sync

## Tools


Types of storage accounts:

1. General Purpose V2 - for blobs, files, queues, tables
2. BlockBlobStorage - premium performance block and append blobs
3. FileStorage - premium performance files

Legacy account types:

1. General Purpose V1 - for blobs, files, queues, tables
2. BlobStorage - Blobs only

Storage Account Redundancy:

- Locally-redundant storage (LRS) - 3 syncronous copies in the **same data center** (but different racks). Protects against rack and drive failures.
- Zone-redundant storage (ZRS) - 3 syncronous copies across availability zones in the **same region**. protects against data center failures.
- Geo-redundant stoage (GRS) is both:
  - LRS (3 different racks)
  - Asyncronous copy in **different region**. The seconday is also LRS, but not available until initiating an account failover.
- Geo-zone-redundant storage (GZRS) is both:
  - ZRS (3 different data centers)
  - Asyncronous copy in **different region**. The seconday is also LRS, but not available until initiating an account failover.
- Read-access geo-zone-redundant storage (RA-GZRS) - Same as GZRS but the secondary available as a read-only access before a failover.

General purpose storage accounts v1 only supports:

1. LRS
2. GRS
3. RA-GZRS

Read more: [Regions and Availability Zones in Azure](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

General Purpose stoage accounts v2 support access tiers: Hot and Cool. Pick the usage pattern; lower storage/higher retrieval or higher storage/lower retrieval

At the Blob level only (not the account level), can also set access tier to Archive.
