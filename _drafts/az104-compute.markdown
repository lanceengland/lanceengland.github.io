---
date: 2020-11-27 -- todo
tags: integration automation
title: "AZ-104 Compute"
---
# AZ-104 Compute

## Virtual Machines

VMSS

Cloud-init.txt - Linux deployment like NGINIX

Redepolying will be allocated to a different hardware cluster. Or, stopping/starting the VM will do the same.

## Azure App Service

Before you can host a web app, you need an app service plan i.e. a managed collection of compute (likely scale sets under the hood).

Key Points:

- App service plan is deployed to a location, and your web app also runs in this location.
- App service plan can host multiple web apps
- The plan defines:
  - OS (Windows or Linux, which affects supported runtimes)
  - Region
  - Number of VM instances
  - Size of VM
  - Pricing tier
  - Free and shared are multi tenant (recommended for dev + test)

The Web App and the App Service Plan needs to be located in the same region.

You can deploy a .Net Core application on either a Windows OS or a Linux OS.

## Containers

AKS
ACI
