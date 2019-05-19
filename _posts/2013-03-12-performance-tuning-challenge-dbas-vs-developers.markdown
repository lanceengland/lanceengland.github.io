---
date: 2013-03-12
title: "Performance Tuning Challenge&#58; DBAs vs Developers"
---
# Performance Tuning Challenge: DBAs vs Developers

DBAs rule, developers drool! No wait, it's the other way around, right? Developers rule, DBAs drool! The truth is, both roles rule, and but they could rule even more by working together. And ultimately that was the message of the excellent presentation by **Kendra Little** [(@Kendra_Little)](http://twitter.com/Kendra_Little)  and **Jeremiah Peschka** [(@peschkaj)](http://twitter.com/peschkaj) at the March 2013 [Atlanta MDF](http://www.atlantamdf.com/) meeting.

Kendra and Jeremiah each took a side, Jeremiah representing the developers and and Kendra representing the DBAs. I don't want to recap every single detail of the presentation, because I wouldn't do it justice without you seeing their lively exchanges and solid arguments. Like every good discussion, very good points were made on both sides. They debated three important topics.

## ORMs: Good or Evil

Object-relational mapping, or data abstraction layers. Developers argue they are agile, abstract away data storage implementation, allow easier testing environments, and allow developers to work without constant "context-switching" between application code and database code.

DBAs argue that ORMs generate terrible SQL that fills up the query cache, kills performance, and ultimately causes problems for which the DBA gets the blame.

THE RESOLUTION

ORMs aren't going away. Many ORM problems are caused by developers not learning the tool correctly, or completely. Also, many ORMs can be customized to map methods to stored procedures, which the DBA can tune. The DBA can help target the "low-hanging fruit" to optimize to big effect.

## Querying the Database: Live vs Cache

Should data always come from the "source of truth", or can things be cached? Developers argue that caching *done well* is hard. Jeremiah put up a quote "There are only two hard things in Computer Science: cache invalidation and naming things". Developers time is valuable. Why continuously pay to implement and maintain caching logic in your code when you can just scale up/scale out the database?

The DBA counter is that the SQL Server license is the most expense piece of the environment, and as such, let it work on the important stuff instead of populating the same "US State" drop-down box over and over and over.

THE RESOLUTION

Cache the easy stuff at the application layer i.e. small things that aren't going to change frequently. Then you can identify frequent queries with DMVs (look for query hash and number of plans for the hash) and determine a database caching strategy.

## Developer Access to Production

Developers argue that if they can't verify a bug, then it's not a bug. And testing in a dev environment just can't simulate real-world usage.

DBAs argue that one incorrect query could kill performance, for which the DBAs take the blame (notice a theme here?). Also, there are data access concerns, even compliance issues. They argue that crafty developers don't *want* access to production; everything can be handed off with deployment scripts and segregation of duties.

THE RESOLUTION

Out of the three topics, this one may have the highest component of "company culture". Issues in production environments affect the company's bottom line, so the stakes are high. As such, teams can get drawn into "turf wars" and finger-pointing when things go wrong. The first step in resolution is to address the culture. This can be easier said than done. After establishing a "we" philosophy, the DBAs can do the following,

- Grant emergency access as needed

- Define roles to limit exposure

- Server-level: View server state

- Database level: View definition, showplan, and specific table access

- Publicize who has access

- Share access to a monitoring tool like [sp_BlitzIndex](http://www.brentozar.com/blitzindex/)

## Summary

"Performance Tuning Challenge: DBAs vs Developers" is a great topic that offers many points of discussion. The things they covered in an hour is just a sample of the topics they will be covering in their [SQL Server Training for Developers](http://www.brentozar.com/services-we-provide/training/sql-server-for-developers/) in Atlanta, GA May 9-10, 2013.

All in all, it was a great night, and Atlanta MDF had a record turnout! Thanks to Kendra and Jeremiah for presenting, to [Brent Ozar Unlimited](http://www.brentozar.com/) for sponsoring, and to Microsoft for hosting. And a big shout out to my friend and co-worker **Don Wert** ([@DonaldWert](http://twitter.com/DonaldWert)), who coordinated the speakers, the pizza, the facility, the swag, and being the MC of the night. Good job, Don!
