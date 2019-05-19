---
date: 2014-07-25
tags: data
title: Safe UPDATE and DELETE
---
# Safe UPDATE and DELETE

It seems like a familiar right of passage for all database people to execute an UPDATE or DELETE statement against an important table while accidentally forgetting the WHERE clause, thereby updating or deleting all rows in the table! What usually follows after the shock and sad realization is the Restore of Shame, where you get to restore the data.

I use a simple trick that helps guard against unfiltered updates/deletes. I certainly didn't invent this technique; I simply thought it would be helpful to share. The more good practices we share with each other, the better for the community.

I use a [common table expression](http://msdn.microsoft.com/en-us/library/ms175972.aspx) (or CTE) to specify the records I want to update/delete. The steps are simple:

- Write a SELECT query to isolate your recordset

- Wrap the SELECT in a CTE

- Execute your UPDATE/DELETE against the CTE

Simple, eh? For the extra cautious, since you can get the expected ROWCOUNT from your SELECT query, you can wrap the statement in a TRANSACTION. Here is an example for AdventureWorks:

First, let's create a "copy" table, so as to not mess up AdventureWorks.

<pre data-enlighter-language="sql">
if OBJECT_ID('Production.Product_temp') is not null
    drop table Production.Product_temp
;
select *
into Production.Product_temp
from Production.Product;
</pre>

Next, write your SELECT statement.

<pre data-enlighter-language="sql">
select *
from Production.Product_temp
where ProductModelID = 23;
</pre>

Then wrap it in a CTE and write your UPDATE statement against it (and not the base table!)

<pre data-enlighter-language="sql">
with cte_safeUpdate as
(
     select *
     from  Production.Product_temp
     where ProductModelID = 23
) update cte_safeUpdate
SET ProductModelID = NULL;</pre>

As I mentioned, you can also wrap a transaction around it

<pre data-enlighter-language="sql">
begin tran;
with cte_safeUpdate as
(
    select *
    from  Production.Product_temp
    where ProductModelID = 23
)
update cte_safeUpdate
SET ProductModelID = NULL;
if @@ROWCOUNT = 10
    commit;
else
    rollback;
</pre>

The concept works the same for DELETE statements.

<pre data-enlighter-language="sql">
with cte_safeDelete as
(
    select *
    from  Production.Product_temp
    where ProductModelID = 23
)
delete from cte_safeDelete;
</pre>

Feel free to share your own tips or suggestions. Hope this helps someone avoid the 'restore of shame'!