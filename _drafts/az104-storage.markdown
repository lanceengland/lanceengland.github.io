---
date: 2020-11-27 --todo
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

## Storage Account Types

| Type                        | Services                                                               | Redundancy                           | Usage                                                                                            |
|-----------------------------|------------------------------------------------------------------------|--------------------------------------|--------------------------------------------------------------------------------------------------|
| General Purpose v2          | Blob (Block (incl. Data Lake), Append, Page) Azure Files, Table, Queue | LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS | Most scenarios. Azure Files only support SMB, hot, cool, and transaction optimized tiers.|
| Premium block blobs         | Blob (incl. Data Lake), Append                                         | LRS, GRS                             | SSD-backed                                                                                       |
| Premium file shares         | Azure Files                                                            | LRS, GRS                             | Supports SMB and NFS. 100 TB, no access tiers.                                               |
| Premium page blobs          | Page Blobs                                                             | LRS                                  | SSD-backed. Premium VHDs.                                                                    |
| General Purpose v1 (Legacy) | Blob (Block, Append, Page), Azure Files, Table, Queue                  | LRG, GRS, RA-GRS                     | Legacy reasons e.g. classic deployment model. No access tiers.|
| Standard Blob (Legacy)      | Blob (Block, Append)                                                   | LRG, GRS, RA-GRS                     | Not recommended by Microsoft, use General Purpose v2 blobs                                       |

Note: General Purpose v2 supports all the functionality of General Purpose v1 and Standard Blob, accounts. Legacy accounts can be upgraded in-place, and are not reversable.

## Storage Tiers

Azure Files storage tiers:

- Premium (highest price and performance, supports both SMB and NFS shares). Provisioned billing (pay per capcity)
- Transaction Optimized (should be named Really Hot; lower transaction, higher storage cost than Hot. SMB share only). Pay as you Go.
- Hot (lower transaction, higher storage cose than Cool. SMB share only). Pay as you Go.
- Cool (lower storage, higher trasaction than Hot. SMB share only). Pay as you Go.

## Lifecycle Management

## Redundancy

Redundancy live migration is only supported for LRS and GRS. Any RA option would have to be removed first (deleting the read-only copy) before migration.

## Access Control

Azure AD -
  RBAC roles:
    Storage File Data SMB Share Reader
    Contributor
    Elevated Contributor

Account Keys

Shared Access Signature

Azure Files via REST supports SAS, but not via SMB.

Control plan vs data plane

## Azure File Sync

- Centralize file shares in Azure. On-prem Windows servers can keep full copies, or act as frequently accessed cache servers.
- Backed by Azure Files with redundancy options
- Simple conflict-resolution strategy: The first change keeps the original file name. Other conflicting changes (up to a max of 100 per file) are kept with a naming convention of \<FileNameWithoutExtension\>-\<endpointName\>[-#].\<ext\> For cloud endpoints, the endpont name will be "cloud".
- Limit one cloud endpoint per Sync group
- A server endpoint can only have one file path

## Tools

Azure Import/Export job (todo: make table)

Import
  Blob (Block, Page)
  Azure Files
Export
  Blob (Block, Page, Append)

Azure Storage Explorer - GUI client

AZCopy - Command-line tool that supports Blob and Files only.

Supported Authorization:

| Type | Supported Authorization |
|------|-------------------------|
| Blob                | Azure AD & SAS          |
| Blob (hierarchical) | Azure AD & SAS          |
| Files               | SAS only |

## Misc

object replication. what is the use case?

change how a storage acct is replicated
https://docs.microsoft.com/en-us/azure/storage/common/redundancy-migration

moving storage accounts to another region

## Properties

Storage Account URL pattern

- _storageAccountName_.**file.core.windows.net**/_fileShareName_
- _storageAccountName_.**blob.core.windows.net**/_containerName_
- _storageAccountName_.**queue.core.windows.net**/_queueName_
- _storageAccountName_.**table.core.windows.net**/_tableName_

---
notes to organize

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

Read more: [Regions and Availability Zones in Azure](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

General Purpose stoage accounts v2 support access tiers: Hot and Cool. Pick the usage pattern; lower storage/higher retrieval or higher storage/lower retrieval

At the Blob level only (not the account level), can also set access tier to Archive.
