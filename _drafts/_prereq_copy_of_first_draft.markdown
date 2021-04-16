# AZ-104 Prerequisites for Azure administrators

Note: this is a temp copy that will be deleted after reorganizing and finishing the final version

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

Network Topologies:

- Bus - physical limitation, a break in bus cable fail entire network
- Ring - Each devic connected to its neighbor. A break in cable would also affect network performance
- Mesh - Physical vs logical. Full physical mesh is super redundant, but complex and rare. Partial mesh is more common. Logical mesh is most modern networks, since each device can connect to all other devices
- Star - Most commonly used. Each network device connects to centralized hub or switch. Switches and hubs can be linked together to extend networks. The most robust and scalable topology.

Network Infrastructure:

- Repeaters - regenerates the data packet
- Hubs - a multiport repeater. Downgrades to slowest device connected. Mostly a home device
- Bridges - divides a network into segments. Filters/forwards data packets between segments using MAC addresses
- Switches - combines the functionality of a bridge and a hub. Managed vs unmanaged. Managed has a host of features (QoS, VLAN, Spanning Tree Protocol (STP), SMTP client, etc).
- Routers - link networks with different ranged addresses together using BGP (typically). Types:
  - Access - Home/small satellite office
  - Distribution - More computing power for more routing info. QoS over WAN
  - Edge - Operates at boundary between networks
  - Core - Optimized for speed and minimizing packet loss

Azure offers ExpressRoute (a dedicated circuit between on-prem on cloud, sold through a partner) and hub-spoke, a reference architecture. A Vnet is the hub, and other Vnets can have peering to the hub. The on-prem spoke is connected by either VPN gateway or ExpressRoute.

Main addresses: MAC and IP

Data packet/datagram - unit of message including header, body, trailer. Data gram is same but usually refers to a more loss-tolerant situation e.g. UDP

Protocol Types:

- Communication - TCP, IP, UDP, HTTP, FTP, SMTP, IMAP, etc
- Security - SSL, TLS, HTTPS, SSH, Kerberos
- Management - SNMP, ICMP

Ports - The first 1024 are well known 25, 80, 443, etc.

Monitor services ini Azure with:

- Azure Network Watcher - capture packets from a service
- Network Performance Monitor - monitor health of entire network
- Performance Monitor - a capability in Network Performance Monitor

## Fundamentals of network security

Server models:

- Request/response - client request a response from server
- Peer to peer - every device is a client and server e.g. BitCoin
- Publish/subscribe - client subscribes to a service

Authentication types:

- Password
- Two-factor
- Token - a specialized two-factor where the code is generated on a device that can be distributed/returned
- Biometric - fingerprint, facial, etc.
- Transactional - looks for unusual characteristics e.g. logging in from other side of world locks account
- Computer recognition - CAPTCHA
- Single sign-on

Kerberos - (source: https://docs.microsoft.com/en-us/learn/modules/network-fundamentals-2/media/3-kerberos.svg)

![Kerberos Diagram](https://docs.microsoft.com/en-us/learn/modules/network-fundamentals-2/media/3-kerberos.svg)

TLS/SSL - (source: https://docs.microsoft.com/en-us/learn/modules/network-fundamentals-2/media/3-tls-ssl.svg)

![TLS/SSL](https://docs.microsoft.com/en-us/learn/modules/network-fundamentals-2/media/3-tls-ssl.svg)

Authentication vs authorization - Authentication happens BEFORE authorization, confirms identity. Authorization confirms actions.

Network security basics: Segment the network into zones (inside, outide, DMZ) and define policies for traffic between the zones.

Firewalls can be hardware or software. They can inspect different levels of OSI model.

Network Security Groups (NSG) - a fundamental component of Azure security.

![NSG](https://docs.microsoft.com/en-us/learn/modules/network-fundamentals-2/media/4-nsg.svg)

Tip: Use the Azure Network Watcher service and enable NSG flow logs (saves JSON file to a storage account).

![Network Security Groups](https://docs.microsoft.com/en-us/learn/modules/network-fundamentals-2/media/4-nsg.svg)

Azure Firewall - a fully managed firewall as a service

VPN can either be site-to-site or point-to-site. Both use a VPN Gateway in Azure.

Network Security Considerations:

- Consider an Azure network security appliance for advanced capabilities
- Convigure Azure services to only connect to VNets, not public internet
- Disable SSH/RDP where possible
- Consider a load balancer to distribute traffic (assuming you have architected for it)

Network Monitoring - can be agent or agentless.

Monitoring Protocols:

- Simple Network Management Protocol (SNMP) - most Linux devices support this
- Windows Management Instrumentation (WMI) - Windows-specifc
- System Logging Protocol (Syslog) - event logging

FCAPS - Fault mgmt, Configuration mgmt, Accounting/administration, Performace mgmt, Security

Azure Monitor - a unifying solution that collects log data for analysis. Can integrate with Application Insights

Log Analytics - a tool in Azure Monitor to query and aggregate large amounts of log data

## Control Azure services with the CLI

Azure resources have a REST API. The API is then used by the CLI, PowerShell, and SDK. Or, the REST API can be called directly.

This module is on the command-line interface (CLI). All the above options are tools for automating and managing.

To get the Azure API on Linux, use: apt-get on Ubuntu, yum on Red Hat, and zypper on OpenSUSE.

Be aware of the shell, as it can affect syntax e.g. variable = "value" in Bash vs variable = "value" in PowerShell.

Commands in the CLI are structured in groups and subgroups. Group = service, subgroups = commands

Good practice: Use "az find" to search for commands e.g. az find (no quotes needed for this one) or az find "az vm" or az find "az vm create"

Use --help convention for info e.g. as storage blob --help

az commands support filtering e.g. az group list --query "[?name == '$RESOURCE_GROUP']" The part after --query is [JMESPath](https://jmespath.org/) syntax, a JSON query language by James Saryerwinnie.

## Automate Azure tasks using scripts with PowerShell

The Az module replaces the AzureRm module. It has some backwards compatability through aliases.

Steps to use locally:

1. Install-Module Az
2. Import-Module Az
3. Connect-AzAccount
4. Get-AzContext - lists active subscription (can only be 'in' one at a time)
5. Get-AzSubscription lists all subscriptions
6. Select-AzSubscription -SubscriptionId '&lt;guid&gt;' to enter a subscription

Get-AzResource - lists all resources. Can also filter by resource group ala Get-AzResource -ResourceGroupName &lt;YourResourceGroupName&gt;

An example creating a VM with Get-Credential: New-AzVM -Name MyVm -ResourceGroupName ExerciseResources -Credential (Get-Credential) ...

You can get an object, modify properties, then pass it to an update cmdlet e.g. Update-AzVM -ResourceGroupName $ResourceGroupName  -VM $vm

Execute a saved PowerShell script: .\myScript.ps1

Define simple function parameters: param([string]$location, [int]$size)

## Secure your identities by using Azure Active Directory

Identity Secure Score - a number between 1 and 223 on how well you match recommendations and best practices from Microsoft for tenant security. Go to Azure AD tennant -> Security -> Identity Secure Score

Azure AD does NOT replace Active Directory; they can be used together, linked through hybrid identity using one of the following authentication methods:

- Azure AD password hash synchronization - password hash stored is stored the cloud
- Azure AD pass-through authentication - on-prem AD handles authentication
- Federated authentication - allows on-prem multifactor authentication (MFA) and smart-card authentications

Plans/Licensing:

- Free - Users, Groups, on-prem Active Directory sync, self-service password reset, single sign-on
- P1 - Free features + Azure AD users can access on-prem resoiurces, dynamic groups, self-serice group mgmt
- P2 - P1 features + Active Directory Identity Protection
- Pay-as-you-Go licenses for specific features (see list below)

Essential Features:

- B2B - invite external users to your tenant
- B2C - business-to-customer identity as a service (uses customer preferred identity mgmt)
- DS - Domain services

Read more: [Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/)

## Introduction to Docker containers

Credit: https://docs.microsoft.com/en-us/learn/modules/intro-to-docker-containers/media/5-multiple-app-isolation.svg

![Conceptual overview](https://docs.microsoft.com/en-us/learn/modules/intro-to-docker-containers/media/5-multiple-app-isolation.svg)

Containers CAN have an OS, which I did not know. I need to research this.

Containers have a persistence layer that is destroyed when the container is removed. Two storage approaches:

1. Volumes - Docker-managed storage on the host filesystem. Multiple comtainers can shre volume. Assumes host will NOT modify.
2. Bind-mounts - mounts a file or directory. Assumes the host WILL modify.

Network:

- Bridge - Default. Internal, private network. Containers can reach other containers by IP
- Host - Runs the container directly on the host network (Linux only)
- None

Drawbacks:

- Containers share a single OS kernal; single-point of failure
- Monitoring becomes more complicated

I believe both of these points (and many others) are addressed by Kubernetes.

## Choose a data storage approach in Azure

Structured vs Semi-structured vs Unstructured

- Structured - Relational + SQL
- Semi-structured
  - Key/value - No query language, simple commands to put/get value by key
  - Graph - Nodes that point to nodes via relationships (not the same as relational)
  - DocumentDb - Store data in any structure by serializing data via JSON, XML, possibly(?) YAML)
- Unstructured - Media, binary, PowerPoint, PDF, log files

Azure storage options:

- CosmosDB - Supports, structured and semi-structured, high-availability, transactional, distributed, low-latency, $$
- Azure SQL Database - Relational
- Azure Table Storage - key/value. Redis is also an option
- Blob storage - unstructured. Can use with content delivery network (CDN) if needed
- Other - HBase, SQL Data Warehouse, Azure Analysis Services, Azure Stream Analytics

Factors to consider when deciding:

- type of data
- operations required
- expected latency
- transactional requirements

## Test Quiz

My learning plan includes taking a WhizLab subscription to their AZ-104 course and practice tests. They offer 4 full practice tests, and a free 15-question test. I took the 15-question test after the finishing the prerequisites path, just to get a barometer of where I need to focus and give me a benchmark to compare to future results. Links for more information on questions missed are below:

- [Azure Load Balancer](https://docs.microsoft.com/en-us/azure/load-balancer/skus)
- [Azure FileSync deployment](https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal%2Cproactive-portal)
- [SLA for VM Scale Sets](https://azure.microsoft.com/en-ca/support/legal/sla/virtual-machine-scale-sets/v1_1/)
- Managed disks do not have an SLA; they depend on the SLA for the [VM](https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_9/) and the underlying [storage account](https://azure.microsoft.com/en-us/support/legal/sla/storage/v1_5/)
- You must have permission to create required virtual network resources in order to create a VM
