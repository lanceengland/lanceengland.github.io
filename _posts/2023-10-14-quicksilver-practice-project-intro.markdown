---
date: 2023-10-14
tags: data automation
title: "Quicksilver Practice Project - Intro"
---
# Quicksilver Practice Project - Intro

This Quicksilver project is simply to get hands-on practice in cloud technologies. Quicksilver is a fictional courier company from an [80s movie](https://www.imdb.com/title/tt0091814/?ref_=nv_sr_srsg_0_tt_5_nm_3_q_quicksilver) (credit to co-worker Bill Delaune for coming up with the idea). The goal is to build a basic front-end client and back-end system for customers and couriers to have packages delivered. The code will be a very basic proof-of-concept, but will be built with DevOps principals of source control, continuous integration (CI) and continuous delivery (CD).

Back-end technologies used:

- Azure DevOps
- Azure SQL Database
- ASP.net API
- Azure Container Instance
- Azure Maps
- Entra ID (aka Azure AD)
- Azure KeyVault

Front-end technologies:

- To be determined, probably some super basic web app

## User Stories

Customer

- Create/edit account
- Create/edit package/delivery request
- Authentication/Authorization

Courier

- Create/edit account
- Create/edit delivery job of package
- Authentication/Authorization

## Implementation

Data will be persisted in Azure SQL Database using an API interface. API will be a C# ASP.net Core container hosted in Azure Container Instance. Azure Maps will be used to translate addresses to Geocodes, and to provide route directions to couriers.

Code will be hosted in either GitHub or Azure DevOps, with CI/CD using Azure Pipelines.

## Plan and Random Thoughts

I'm starting with my wheelhouse, the data model. However, my inital model will be incomplete by design, so that I can then hook up a CI/CD pipeline for further devlopment.

Next, I will implement a very basic API to for CRUD operations against the data model. Again, because the data model will be incomplete, this will also evolve with CI/CD.

I need to determine where and how to handle authentication/authorization for the API. I could put an API Management in front of it, but also want to explore other options.

I prefer the repo to be in GitHub, but will initially use Azure Repos to simplify implementing the pipelines in Azure DevOps. Later, I may try moving the repo to GitHub and either intergrating with Azure DevOps for the pipelines, or use GitHub actions. Or, maybe both just for the experience.

I will use VSCode, including the [SQL Database Project](https://marketplace.visualstudio.com/items?itemName=ms-mssql.sql-database-projects-vscode) extension, and for the API use the [Developing Inside a Container](https://code.visualstudio.com/docs/devcontainers/containers) functionality.

Infrastructure-as-Code will be defined with [Bicep](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview?tabs=bicep) files.

The development and production pipelines will target different resources, optimized for cost in development and optimized for scaling in production, while still aiming for cost effectiveness.

---

The Quicksilver Practice Project Series:

- **Intro**
- [Database](2023-10-18-quicksilver-practice-project-database)
- API (Coming soon)
