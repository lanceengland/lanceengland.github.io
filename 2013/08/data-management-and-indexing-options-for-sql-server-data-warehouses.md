# Data Management and Indexing Options for SQL Server Data Warehouses


<img src="/s/AtlantaMDF_logo_Horizontal.jpg" style="float:left;display:inline;padding:2em">Thanks to everyone that attended my presentation at Atlanta MDF last night. It was great meeting some new people after the talk, and it seemed like it was well received. Here are the links to the [slidedeck ](/s/mgmt_idx_dw_slides-3kmm.pptx)and [demo scripts](/s/mgmt_idx_dw_demos-t687.zip). Remember, the scripts were written for [AdventureWorksDW for SQL Server 2012](http://msftdbprodsamples.codeplex.com/releases/view/105902).



Also, I included the links section below as well because it's a handy reference that I will continue to use in the future :) 



If anyone has any questions or additional comments (even constructive criticisms) I'd love to hear from you. Comment below, or reach out via [Twitter](http://twitter.com/lanceengland) or [LinkedIn](http://www.linkedin.com/in/lanceengland).


<hr/>

<h2 id="links">LINKS

<h3 id="datawarehouse">DATA WAREHOUSE


Microsoft EDW Architecture, Guidance and Deployment Best Practices <br />
[http://msdn.microsoft.com/en-us/library/hh147624.aspx](http://msdn.microsoft.com/en-us/library/hh147624.aspx)



The Data Loading Performance Guide <br />
[http://msdn.microsoft.com/en-us/library/dd425070(v=sql.100).aspx](http://msdn.microsoft.com/en-us/library/dd425070(v=sql.100).aspx)



Top 10 Best Practices for Building a Large Scale Relational Data Warehouse <br />
[http://sqlcat.com/sqlcat/b/top10lists/archive/2008/02/06/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse.aspx](http://sqlcat.com/sqlcat/b/top10lists/archive/2008/02/06/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse.aspx)


### PARTITIONED TABLES  


Partitioned Table and Index Strategies Using SQL Server 2008 <br />
[http://technet.microsoft.com/en-us/library/dd578580%28v=sql.100%29.aspx](http://technet.microsoft.com/en-us/library/dd578580%28v=sql.100%29.aspx)



Strategies for Partitioning Relational Data Warehouses in Microsoft SQL Server <br />
[http://technet.microsoft.com/en-us/library/cc966457.aspx](http://technet.microsoft.com/en-us/library/cc966457.aspx)



Table Partitioning Resources <br />
[http://www.brentozar.com/sql/table-partitioning-resources/](http://www.brentozar.com/sql/table-partitioning-resources/)



Query Processing Enhancements on Partitioned Tables and Indexes <br />
[http://msdn.microsoft.com/en-us/library/ms345599.aspx](http://msdn.microsoft.com/en-us/library/ms345599.aspx)


<h3 id="indexedviews">INDEXED VIEWS


Creating Indexed Views (long list of restrictions) <br />
[http://msdn.microsoft.com/en-us/library/ms191432.aspx](http://msdn.microsoft.com/en-us/library/ms191432.aspx)



Partition Switching with Indexed Views <br />
[http://technet.microsoft.com/en-us/library/bb964715(v=sql.105).aspx](http://technet.microsoft.com/en-us/library/bb964715(v=sql.105).aspx)


<h3 id="filteredindexes">FILTERED INDEXES


Filtered Index Design Guidelines <br />
[http://msdn.microsoft.com/en-us/library/cc280372(v=sql.100).aspx](http://msdn.microsoft.com/en-us/library/cc280372(v=sql.100).aspx)


<h3 id="datacompression">DATA COMPRESSION


Data Compression: Strategy, Capacity Planning and Best Practices <br />
[http://msdn.microsoft.com/en-us/library/dd894051%28v=sql.100%29.aspx](http://msdn.microsoft.com/en-us/library/dd894051%28v=sql.100%29.aspx)


<h3 id="columnstoreindexessql2012">COLUMNSTORE INDEXES (SQL 2012)


Columnstore Indexes BOL <br />
[http://msdn.microsoft.com/en-us/library/gg492088.aspx](http://msdn.microsoft.com/en-us/library/dd894051%28v=sql.100%29.aspx)



Columnstore Internals <br />
[http://rusanu.com/2012/05/29/inside-the-sql-server-2012-columnstore-indexes/](http://rusanu.com/2012/05/29/inside-the-sql-server-2012-columnstore-indexes/)



SQL Server Columnstore Performance Tuning <br />
[http://social.technet.microsoft.com/wiki/contents/articles/4995.sql-server-columnstore-performance-tuning.aspx](http://social.technet.microsoft.com/wiki/contents/articles/4995.sql-server-columnstore-performance-tuning.aspx)


<h3 id="clusteredcolumnstoreindexessql2014">CLUSTERED COLUMNSTORE INDEXES (SQL 2014)


Clustered Columnstore Internals <br />
[http://rusanu.com/2013/06/11/sql-server-clustered-columnstore-indexes-at-teched-2013/](http://rusanu.com/2013/06/11/sql-server-clustered-columnstore-indexes-at-teched-2013/)



Improvements in Batch-mode support (TechEd Video) <br />
[http://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DBI-B322#fbid=rAFPjiEmlNt](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DBI-B322#fbid=rAFPjiEmlNt)
 
