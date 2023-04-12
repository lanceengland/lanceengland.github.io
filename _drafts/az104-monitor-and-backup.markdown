---
date: 2020-11-27 -- todo
tags: integration automation
title: "AZ-104 Monitor and Backup"
---
# AZ-104 Monitor and Backup

## Monitor

Azure has out-of-the-box platform logs automatically generated for each resource. The types of platform logs:

- Activity log - Management plane, who, what, when. Can be viewed in portal
- Resource log - Data plane, operations performed within the resource (previously called diagnostic logs). Requires diagnostic setting to send to a destination.
- Azure AD logs - Sign-in and audit trail for a tenant in Azure AD. Viewable in the portal.

> A disgnostic setting can send platform logs to Log Analytics workspace, Event hub, Azure Storage, or Azure Monitor partner integrations.

Azure Monitor is a centralized blade for collecting two types of data, metrics (numeric) and logs (text).

Most resources have a monitoring section of the menu to view individual metrics and log data. For example, a VM. Or, in Azure Monitor, you can view collective metrics or search through collective logs.

Features:

- Application Insights - app performance monitoring through agent or SDK
- VM Insights - VM and VMSS. Steps to configure:
  - Log Analytics workspace
  - Add VMInsights to the workspace
  - Install agents on the VM and VMSS
  - To enable guest OS insights, install Azure Monitor agent and enable data collection rule. Previously done with Azure diagnostic extension for Windows (WAD) and Linux (LAD).
- Container Insights - K8s in AKX, ACI, Azure Stack, Arc-Enabled. Uses a containized version of Log Analytics agent for Linux that sends metrics to Azure Monitor metrics db, and logs to Log Analytics workspace.
- Log Analytics - data workspace, logically stored as tables/columns. Pay for ingestion and storage. More notes below.
- Metrics Explorer (interactive visualizations, i.e. embedded Power BI)
- Supports Kusto query language

### Agents

- Azure Monitor agent (new)
- Azure Diagnostics (Windows)
- Log Analytics agent (which supports VM Insights)
- Telegraf (Linux)

> Azure Monitor agent will replace Log Analytics, Telegraf, and Diagnostics

NSGs can have diagnostics! Need notes from practice test.

Linux Diagnostic Extension (LAD) 3.0: https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/diagnostics-linux

Overview of Azure Monitor Agents: https://docs.microsoft.com/en-us/azure/azure-monitor/platform/agents-overview

Azure Performance Diagnostics: https://docs.microsoft.com/en-us/azure/virtual-machines/troubleshooting/performance-diagnostics

### Log Analytics workspace

A Log Analytics workspace can be in any region and does NOT need to be in the same region as the recovery services vault.

Kusto is a language that looks a little like LINQ, and can be used to search, filter, and aggregate log data.

### Other

Azure Advisor - a service to recommend best practices and cost optimizations

Service Health - a service is used to inform users on the health of all Azure based services

## Backup

Azure Backup is implemented with backup policies. A backup policy can be created for the following types:

- Azure Virtual
- Azure File Share
- SQL Server in Azure VM
- SAP HANA in Azure VM

### Azure Site Recovery

### Recovery Services Vault

The storage account used by RSV needs to be in the same region/location.

File recovery
