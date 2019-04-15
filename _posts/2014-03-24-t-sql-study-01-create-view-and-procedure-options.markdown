---
date : 2014-03-24 01:32:24
tags: data
---
# T-SQL Study: Create View and Procedure Options

It's time to work on some goals for 2014. Obtaining a SQL Server certification is something I had planned to pursue last year, but am determined to tackle it this year. The first test is [70-461: Querying Microsoft SQL Server](http://www.microsoft.com/learning/en-us/exam-70-461.aspx). For each test, I will review the high level objectives, jot down the areas I am weak in, and then blog about it. When I'm finished I will have a collection of study notes and will also have increased my blog post count for this year. That, my friends, is called efficiency ;)

The maiden topic is pretty basic: Reviewing some of the options for creating views and stored procedures.

## CREATE VIEW

[http://technet.microsoft.com/en-us/library/ms187956.aspx](http://technet.microsoft.com/en-us/library/ms187956.aspx)

Notes: A view can be created only in the current database. The CREATE VIEW must be the first statement in a query batch. A view can have a maximum of 1,024 columns.

### SCHEMA BINDING

Binds the view to the underlying objects if references such that no changes are allowed to referenced objects unless you drop  or alter the view to not use schema binding.
ENCRYPTION
Encrypts the contents from displaying in sys.syscontents. Also, does not publish with replication.
VIEW METADATA
Presents the column metadata as belonging to the view i.e. masking the base tables. Useful to assist ORM tools from getting confused walking the structure of data returned.
CHECK
Used when updating data through a view. Forces update statements to follow the criteria set within the underlying SELECT statement. In other words, if an UPDATE causes a row to disappear from the view, the UPDATE fails.

## CREATE PROCEDURE

[http://technet.microsoft.com/en-us/library/ms187926.aspx](http://technet.microsoft.com/en-us/library/ms187926.aspx)

### ENCRYPTION

Like with CREATE VIEW, obfuscates the procedure definitiontext. However, the text is viewable by privileged users who can access system tables. Cannot be published with replication.

### EXECUTE AS

Specifies the security context under which to execute the procedure.

### OWNER

The current owner of the module (or schema if blank)

### SELF

Equivalent to EXECUTE AS [user], where the specified user is the person creating or altering the module

### CALLER

The default for all modules except queues. Executes under the context of the person clling the module (duh).

### USER

Specifies a specific user. Note: Must be a singleton account i.e. *no group, role, or built-in account*

### RECOMPILE

Instructions to not cache the query plan. Cannot be published with replication. Used to counteract "parameter-sniffing". More information: [http://technet.microsoft.com/en-us/library/ms190439.aspx](http://technet.microsoft.com/en-us/library/ms190439.aspx)

The first post in a series of T-SQL posts on new and/or unfamiliar features in preparation for taking the 70-461 exam.