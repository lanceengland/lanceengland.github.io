---
date : 2013-08-13 05:40:13
---
# Data Management and Indexing Options for SQL Server Data Warehouses

![Atlanta MDF](/assets/img/atlantamdf.jpg)

Thanks to everyone that attended my presentation at Atlanta MDF last night. It was great meeting some new people after the talk, and it seemed like it was well received. Here are the links to the [slide deck and demos](/assets/presentations/data_mgmt_atlantamdf.zip). The scripts were written for [AdventureWorksDW for SQL Server 2012](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks).

Also, I included the links section below as well because it's a handy reference.

If anyone has any questions or additional comments (even constructive criticisms) I'd love to hear from you. Comment below, or reach out via [Twitter](http://twitter.com/lanceengland) or [LinkedIn](http://www.linkedin.com/in/lanceengland).

## LINKS

### DATA WAREHOUSE

Microsoft EDW Architecture, Guidance and Deployment Best Practices: [http://msdn.microsoft.com/en-us/library/hh147624.aspx](http://msdn.microsoft.com/en-us/library/hh147624.aspx)

The Data Loading Performance Guide: [http://msdn.microsoft.com/en-us/library/dd425070(v=sql.100).aspx](http://msdn.microsoft.com/en-us/library/dd425070(v=sql.100).aspx)

Top 10 Best Practices for Building a Large Scale Relational Data Warehouse: [http://sqlcat.com/sqlcat/b/top10lists/archive/2008/02/06/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse.aspx](http://sqlcat.com/sqlcat/b/top10lists/archive/2008/02/06/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse.aspx)

### PARTITIONED TABLES  

Partitioned Table and Index Strategies Using SQL Server 2008: [http://technet.microsoft.com/en-us/library/dd578580%28v=sql.100%29.aspx](http://technet.microsoft.com/en-us/library/dd578580%28v=sql.100%29.aspx)

Strategies for Partitioning Relational Data Warehouses in Microsoft SQL Server: [http://technet.microsoft.com/en-us/library/cc966457.aspx](http://technet.microsoft.com/en-us/library/cc966457.aspx)

Table Partitioning Resources: [http://www.brentozar.com/sql/table-partitioning-resources/](http://www.brentozar.com/sql/table-partitioning-resources/)

Query Processing Enhancements on Partitioned Tables and Indexes: [http://msdn.microsoft.com/en-us/library/ms345599.aspx](http://msdn.microsoft.com/en-us/library/ms345599.aspx)

### INDEXED VIEWS

Creating Indexed Views (long list of restrictions): [http://msdn.microsoft.com/en-us/library/ms191432.aspx](http://msdn.microsoft.com/en-us/library/ms191432.aspx)

Partition Switching with Indexed Views: [http://technet.microsoft.com/en-us/library/bb964715(v=sql.105).aspx](http://technet.microsoft.com/en-us/library/bb964715(v=sql.105).aspx)

### FILTERED INDEXES

Filtered Index Design Guidelines: [http://msdn.microsoft.com/en-us/library/cc280372(v=sql.100).aspx](http://msdn.microsoft.com/en-us/library/cc280372(v=sql.100).aspx)

### DATA COMPRESSION

Data Compression: Strategy, Capacity Planning and Best Practices: [http://msdn.microsoft.com/en-us/library/dd894051%28v=sql.100%29.aspx](http://msdn.microsoft.com/en-us/library/dd894051%28v=sql.100%29.aspx)

### COLUMNSTORE INDEXES (SQL 2012)

Columnstore Indexes BOL: [http://msdn.microsoft.com/en-us/library/gg492088.aspx](http://msdn.microsoft.com/en-us/library/dd894051%28v=sql.100%29.aspx)

Columnstore Internals: [http://rusanu.com/2012/05/29/inside-the-sql-server-2012-columnstore-indexes/](http://rusanu.com/2012/05/29/inside-the-sql-server-2012-columnstore-indexes/)

SQL Server Columnstore Performance Tuning: [http://social.technet.microsoft.com/wiki/contents/articles/4995.sql-server-columnstore-performance-tuning.aspx](http://social.technet.microsoft.com/wiki/contents/articles/4995.sql-server-columnstore-performance-tuning.aspx)

### CLUSTERED COLUMNSTORE INDEXES (SQL 2014)

Clustered Columnstore Internals: [http://rusanu.com/2013/06/11/sql-server-clustered-columnstore-indexes-at-teched-2013/](http://rusanu.com/2013/06/11/sql-server-clustered-columnstore-indexes-at-teched-2013/)

Improvements in Batch-mode support (TechEd Video): [http://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DBI-B322#fbid=rAFPjiEmlNt](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DBI-B322#fbid=rAFPjiEmlNt)
