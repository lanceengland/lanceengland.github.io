---
date: 2023-10-18
tags: data automation
title: "Quicksilver Practice Project - Database"
---
# Quicksilver Practice Project - Database

This post is part of a series:

- [Intro]({% post_url 2023-10-14-quicksilver-practice-project-intro %})
- **Database**
- API (Coming soon)

---

Data will be stored in an relational database, specifically an Azure SQL Database. The data model should be fairly stright-forward. My initial model will be intentionaly bare-bones so I can implement a CI/CD pipeline and then add more to it.

## Entities

*Customer* - id, first, last, email. The person requesting a delivery.

*Package* - id, name, size, weight. The item to be delivered.

*Delivery* - id, address, geocode. The destination for the package.

*Courier* - id, first, last, email. The person delivering the package.

Future work will include events (e.g. picked up, out-for-delivery, delivered).

## Questions

Before I get started on design and coding, I have some questions that I will hopefully answer as I'm working through it.

- Authentication/Authorization: This could be a good use case for Azure B2C (business-to-customer). How do I integrate that into the Customer and Courier table, and enforce row-level security?
- Does the VSCode SSDT extension work like the Visual Studio SSDT projects? Does it create a DACPAC that can be deployed? Does it support publish profiles?
- How difficult will it be to implement an Azure SQL Database with a Bicep file?

## Getting Started

### Development

First, I opened VSCode and made sure the SQL Database Projects extension was installed. Then opened the Command Pallette (Ctrl+Shift+P) and choose "Database Projects: New" which then gave an option of Azure SQL Database or SQL Server. I'm not exactly sure what the difference means in terms of the extension functionallity, probably the deployment method(?).

Right-clicking he extension toolbar gives options for New Table, New Stored Procedure, etc. so this looks familiar enough to quickly stub out a handful of tables so I can move to the deployment step.

### Deployment

I'm going to try this multiple ways, each time getting closer to the goal of CI/CD pipeline deployment.

#### From VSCode

First, I'll manually create an Azure SQL Database through the portal and deploy the code from VSCode, to verify the DACPAC deploys as expected.

Azure SQL Databases now have a Free tier perfect for these types of learning excercises. I icked my Entra ID/Azure AD account as the administrator. After the database was created, I set the Server Firewall rule to allow y client IP address.

In VSCode, I choose 'Publish' and it launched a basic wizard in the command pallette area, asking for fully-qualified server name, database name, and authentication method. It also allowed me to save a publish profile. Everything worked, so on to the next deployment method.

#### From Azure DevOps Pipeline

From the Azure portal, I dropped/re-created a new empty database. Note: In order to apply the free offer again, you have to delete the logical server too.

Next, I created a new project in Azure DevOps. Back in VSCode, I used the Git extension to initialize a repo. Under Repos in Azure DevOps, I got the URL of the repo and added it as a Remote for my local git repo (the database project) and published it up to Azure Repos.

Next step, creating the pipeline and setting it up to publish to Azure SQL Database. First challenge, getting the build path correct. This took multiple attempts, including using echo $() to see the path and finding this article on [Building Git Repos](https://learn.microsoft.com/en-us/azure/devops/pipelines/repos/azure-repos-git?view=azure-devops&tabs=yaml#checkout-path) that mentions the default checkout location of /s. Finally was able to build it with the DotNetCoreCLI@2 task.

> Moving on to deployment, at time of writing (October 2023) the SqlAzureDacpacDeployment@1 task only works on Windows agents, so be sure your YAML pipeline targets **vmImage: windows-latest**.

Azure DevOps needs a way to deploy to your Azure sunscription, so first you need to set up an Azure Resource Manager [Service Connection](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops&tabs=yaml). The first option is selecting the authentication type that Azure DevOps (AdO) will use when connecting to the Azure subscription. There are six (6) options, and I only have knowledge of two (2) of them, so that is another area to learn more. I choose **Service principle (Automatic)* in which AdO will create a Service Principle in the related Azure AD aka Entra ID instance. It allows you pick the scope between Management Group, Subscription, and Resource Group, and assigns the Contributor RBAC role.

My first decision point before creating the pipeline was how much Infrastructure-as-Code to automate. Should I try to automate the creation of the Azure SQL Database/logical server instance in the pipeline? When learning something new, I tend to go for the lowest "friction" route at first; I'd prefer to get a minimum-viable concept working, and can improve/productionize it later. So, I decided to create the Azure SQL Database manually using the [free tier](https://learn.microsoft.com/en-us/azure/azure-sql/database/free-offer?view=azuresql).

Next step, SQL Database permissions. The Subscription-level RBAC Contributor role is for management-plane permissions. In order to allow the AdO service principle to create objects inside the database, it must have some level of SQL Server permissions, which I assigned using T-SQL statements after connecting SQL Management Studio. 

> Note: In the Azure portal, browse to the SQL Database and choose "Set Firewall Rules" and choose the option to allow your client IP address.

I created a database user mapped to a server login, then assign the role of **db_owner**. In the future, this could be reduced to some combination of ddl_admin, db_datawriter, etc. However, if you plan on defining custom database roles you'' need to have db_owner or db_securityadmin.

To build a pipeline to deploy the SQL Database Project, I used two tasks:

- [DotNetCoreCLI@2](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/dotnet-core-cli-v2?view=azure-pipelines) - to build the project and produce a DACPAC
- [SqlAzureDacpacDeployment@1](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/sql-azure-dacpac-deployment-v1?view=azure-pipelines) - to deploy the DACPAC against the target Azure SQL Database

It took a multiple attempts to get the file paths figured out. But these are exactly the type of things you just have to fumble through and learn. I used the [system-variable](https://learn.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml) Agent.BuildDirectory and through runing a script task DIR, I discovered my repo code was under /s. After the fact I discovered a different system-variable, Build.Repository.LocalPath, that gives the path to the repo.

After some trial and error, and was able to build and deploy, hurrah! My YAML pipeline (at this point):

<pre data-enlighter-language="yaml">
trigger:
- main

pool:
  vmImage: windows-latest

steps:
- task: DotNetCoreCLI@2
  inputs:
    command: 'build'
    projects: '**/*.sqlproj'

- task: SqlAzureDacpacDeployment@1
  inputs:
    azureSubscription: 'Visual Studio Enterprise Subscription – MPN(guid redacted)'
    AuthenticationType: 'servicePrincipal'
    ServerName: 'quicksilver-dev-sqlsrv01.database.windows.net'
    DatabaseName: 'QuicksilverDb'
    deployType: 'DacpacTask'
    DeploymentAction: 'Publish'
    DacpacFile: '$(Build.Repository.LocalPath)\s\Quicksilver\bin\Debug\Quicksilver.dacpac'
    IpDetectionMethod: 'AutoDetect'
</pre>

## Future Steps

- Change the pipeline to trigger off merging a change request.
- Add parameters for values that will change between environment (subscription, resource group, etc.).
- Learn about different Service Principle authentication options.
- Determine how much Infrastructure-as-Code whould be in the Pipeline. For databases, it's harder to define than application code.
- Reduce the permissons of the service principle in the database, ddl_admin, etc.
- Research the Database Project publish profile options
- Add a task tfor SSDT to generate scripts and add a manual review/approval deployment gate.
- Learn the differences in between the two VSCode Database Project types, Azure SQL Database vs. SQL Server.
