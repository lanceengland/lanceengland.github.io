---
date: 2020-11-27
tags: integration automation
title: "AZ-104 Networking"
---
# AZ-104 Networking

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

Properties of a NSG rule:

- Name - unique
- Priority - lower numbers processed first
- Source or destination IP address(es)
- Protocol  e.g. UDP, TCP
- Direction - inbound or outbound
- Port range - port or range
- Action - Allow or deny


Virtual networks are isolated from each other. You can connect them with [virtual network peering](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview), which uses Microsoft backbone infrastucture and never flows over the internet. The virtual networks do NOT need to be in the same region.

