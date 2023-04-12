---
date: 2020-11-27 --todo
tags: integration automation
title: "AZ-104 Networking"
---
# AZ-104 Networking

todo
CIDR
DNS records cheetsheet

common port numbers
service endpoints, private endpoints

## Network Watcher

Netwrok Watcher is a suite of tools to diagram, monitor, diagnose, enable/disable logging for IaaS products (VMs, VNets, App Gateways, Load Balancers, NSGs) but specifically NOT PaaS products (Azure SQL Database, Azure Firewall, etc).

| Tool                     | Description                                                      |
|--------------------------|------------------------------------------------------------------|
| Topology                 | Generate a downloadable (SVG) visual diagram of the resources in a virtual network. |
| Connection Monitor       | Tracks connection reachability at regular intervals, informs when connection is down, and captures metrics.                                   |
| IP Flow Verify           | 5-tuple connectivity check from a VM                             |
| Next Hop                 | Specify source and target IP address, then tests connectivity and informs what type of next hop is used to route traffic.|
| Effective Security Rules | Results of NSGs                                                  |
| VPN Troublehoot          | Logging of VPN Gateway                                           |
| Packet Capture           | Capture frames to analyze (e.g. Wireshark) to a storage account, the VM, or both.|
| Connection Troubleshoot  | Test a connection between a VM and another VM, an FQDN, a URI, or an IPv4 address. Similar to conection monitor, but tests point in time.|

## Azure Bastion

A PaaS provisioned inside your virtual network. Bastion lets you connect to your VMs to establish RDP or SSH sessions through HTML5 browsers through a single public IP (as opposed to IP-per-VM). Essentially a browser-based jump-box-as-a-service.

## Service Endpoints

Service Endpoints allow your resources (e.g. deployed VMs) to connect to Azure services (e.g. Azure SQL Database) over the Microsoft backbone instead of the public internet.

## Site-to-Site VPN Gateway

Site-to-Site VPN connection needs a gateway subnet named "GatewaySubnet" of /27 or larger. The networks should not have over-lapping IP address ranges.

## Point-to-Site VPN Gateway

Windows clients can access directly peered VNets, but the VPN client must be downloaded again if any changes are made to VNet peering or the network topology.

Non-Windows clients can access directly peered VNets.

Access is not transitive and is limited to only directly peered VNets.

## Public IPs

These do NOT require a virtual network to be created.

## Route Tables

These do NOT require a virtual network to be created.

## Network Interface Card (NIC)

NICs can only be assigned to virtual networks in the same subscription and location. Attaching to a VM is also restricted to the same subscription and location.

## Load Balancers

Basic vs Standard SKU (todo)

You can only attach virtual machines in the same region and that have a standard SKU public IP configuration or no public IP configuration. All IP configurations must be on the same virtual network

VMs in the backend pool must have:

- Static private IP addresses
- NSG to permit traffic

> Remember: STANDARD SKU load balancers requires VMs with STANDARD SKU IP (or no IP config.) Also, the IPs must be in the same VNet.

---
notes to organize

## Virtual Networks

Good practice - VNEt's /16 and subnets /24.

Good practice - Ensure subnets don't cover the entire vnet address space; leave room for future use.

Resources in a VNET can connect outbound to the internet by default.

Make sure your on-site and virtual network IP ranges don't overlap.

The subnet address space can be modified when it is empty. Once a resource e.g. a VM, is assigned the address space is fixed.

Network interface care aka NIC is assigned to a resource, and gets a pulic and private IP address. VMs must be deallocated to add a NIC. A VM can have multiple NICs.

You can disable the public IP on the NIC.

## Network Security Groups

[Network security groups](https://docs.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview) (NSG) can be assigned to a NIC and or a subnet.

NSGs filter traffic. with allow/deny and inbound/outbound rules. Rules have a priority. Lower priority is evaluated first. NSG come with three default, immutable rules. The three defaul rules are 1) allow inbound traffic from the vnet 2) allow inbound traffic from a load balancer, and 3) deny all inbound traffic.

If more than one NSG applies to a resource, then the one closest to the resource takes priority. For example, a NSG applied to a NIC takes priority over a NSG applied the subnet the NIC is attached to.

Properties of a NSG rule:

- Name - unique
- Priority - lower numbers processed first
- Source or destination IP address(es)
- Protocol  e.g. UDP, TCP
- Direction - inbound or outbound
- Port range - port or range
- Action - Allow or deny


Virtual networks are isolated from each other. You can connect them with [virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview), which uses Microsoft backbone infrastucture and never flows over the internet. The virtual networks do NOT need to be in the same region.
