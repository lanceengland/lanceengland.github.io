---
date: 2015-05-12
tags: analysis
title: Datazen Technical Overview
---
# Datazen Technical Overview

With Microsoft's [recent acquisition of Datazen software](https://blogs.microsoft.com/blog/2015/04/14/microsoft-acquires-mobile-business-intelligence-leader-datazen/) I took some time to read through the [documentation](http://www.datazen.com/docs/) on the Datazen website and play with their demos. I am impressed with the product and want to dig deeper into it in the near future. I decided to write up a quick technical overview for future reference.

At a high-level, Datazen consists of three product elements:

- Viewer Apps

- Publisher App

- Datazen Server

## Viewer Apps

Datazen was created with a "mobile-first" mentality, allowing dashboard to be created with different screen size templates for the different mobile devices it supports. All Datazen dashboards can be accessed through its HTML5 interface. In addition, Datazen has native viewer apps for iOS, Android, Windows 8, and Windows Phone.

## Publisher App

The Publisher App is for designing and publishing your dashboards and KPIs. Currently, this tool is only supported on Windows 8.

## Datazen Server

The Datazen Server is the center of the deployment architecture. The server is used for connecting to enterprise data sources, publishing dashboards and KPIs, and optionally integrating to Active Directory for user authentication. The Datazen Server has three components, each of which can be on the same machine, or on different machines for scalability:

- Core Service

- Data Acquisition Service

- Web Applications

### Core Service

The Core Service holds a repository of users, KPIs, dashboards, permissions, and cached data. All data in the repository is encrypted at rest. The repository is built on RavenDB, an open-source document database for Microsoft's .Net platform, which supports full database encryption.

### Data Acquisition Service

The Data Acquisition Service periodically queries external data sources and caches results in the Core Service Repository. The service can access a variety of data sources including Microsoft SQL Server, Microsoft Analysis Server, Oracle, MySQL, PostgresSQL, Microsoft Azure SQL Database, SharePoint lists, Excel, text files, OData feeds, and XML web services.

### Web Applications

The Datazen server contains a number of web applications:

- Control-Panel - Browser-based server administration

- REST-based API - Used by all client apps (iOS, Android, Windows). Client apps never call the data sources directly, only the API.

- Web portal - For accessing HTML5-rendered dashboards and KPIs

All end-points support HTTPS to encrypt data in transit.

## Caching

One of the key features of the Datazen architecture is the use of caching frequently-accessed items. While Datazen supports both connected and disconnected use-cases, their default paradigm is based on disconnected dashboards as mobile usage continues to skyrocket. Datazen supports caching at the server-side, and the client side:

### Server-side Caching

Caching can be configured for the Web API application, the Web Viewer application, or both. The Web API and Web Viewer applications can utilize an in-memory cache (for single instance or testing purposes), or a shared cache service (memcached, redis or Azure Cache).

### Client-side Caching

The challenge with mobile clients is their connectivity situation is inherently unstable, switching connections, bad signals, or jumping between cellular and Wi-Fi. To meet the challenge, Datazen supports two kinds of dashboards: Real-time and Cached.

### Real-time

Real-time dashboards are configured to never cache data locally and always load from the server, or dashboards based on very large data sources which load data fragments on demand as the user interacts with the dashboard.

### Cached

Cached dashboards sync the entire dataset to the device and don’t require server communication to support user interactions. While the device is connected local data is kept in sync with live data sources. The device can lose connectivity at any time and dashboards will continue to function based on the local data. The background data sync happens on a user-specified interval. The sync checks for new versions of all applicable dashboards and their related data views. It only downloads definitions and data views that have changed since the last check, so it is a lightweight operation.

## Summary

I think this is a great looking tool well-worth learning more about. Currently, the pricing model requires SQL Server Enterprise Edition with active Software Assurance. That is a pretty steep barrier for smaller companies, in my opinion. And it will also be very interesting to see how/if they will blend this into their Power BI offering.