---
date: 2017-05-08
tags: data
title: PIVOT and UNPIVOT quick reference
---
# PIVOT and UNPIVOT quick reference

The T-SQL relational operators PIVOT and UNPIVOT are super useful, but I use them *just* infrequently enough where I can never remember the exact syntax from memory. I decided to create this post for a quick reference for myself, and maybe someone will also find it handy.

First off, what do PIVOT and UNPIVOT do? PIVOT will turn row values into columns, and UNPIVOT can turn columns into row values. It's easier to visualize by following the examples below. I wanted the examples to be a quick copy/paste/run into SQL Server Management Studio, so the source table is a hard-coded inline table from a [VALUES](https://docs.microsoft.com/en-us/sql/t-sql/queries/table-value-constructor-transact-sql) table constructor.

## Initial Table

The first query shows the initial data set.

<pre data-enlighter-language="sql">
SELECT
  T.Name,
  T.Val1,
  T.Val2
FROM
  (VALUES
  ('Apple', 'A1', 'A2'),
  ('Banana', 'B1', 'B2'),
  ('Cat', 'C1', 'C2')
  ) AS T(Name, Val1, Val2)
;
</pre>

![result set 1](/assets/img/resultset01.png)

## UNPIVOT

This query takes the table-valued constructor from the first query and UNPIVOTS it.

<pre data-enlighter-language="sql">
SELECT
  UNPVT.Name,
  UNPVT.NEW_COLUMN_NAME,
  UNPVT.EXISTING_COL_NAME
FROM
  (VALUES
  ('Apple', 'A1', 'A2'),
   'Banana', 'B1', 'B2'),
   'Cat', 'C1', 'C2')
 ) AS T(Name, Val1, Val2)
UNPIVOT
  (NEW_COLUMN_NAME FOR EXISTING_COL_NAME IN (Val1, Val2)) AS UNPVT;
</pre>

![result set 1](/assets/img/resultset02.png)

## PIVOT

This query demonstrates PIVOT. It uses a hard-coded equivalent of the result set of query #2 and PIVOTS it back to the original form (embedding an UNPIVOT of the VALUES table-constructor started to look a little too busy and took the focus away from the PIVOT syntax).  Note that an aggregate operator is needed, and the FOR clause matches the FOR clause in the UNPIVOT example.

<pre data-enlighter-language="sql">
SELECT
  Pvt.Name,
  Pvt.Val1,
  Pvt.Val2
FROM
  (VALUES
  ('Apple', 'A1', 'Val1'),
  ('Apple', 'A2', 'Val2'),
  ('Banana', 'B1', 'Val1'),
  ('Banana', 'B2', 'Val2'),
  ('Cat', 'C1', 'Val1'),
  ('Cat', 'C2', 'Val2'))
  AS T(Name, NEW_COLUMN_NAME, EXISTING_COL_NAME)
PIVOT (
  MIN(NEW_COLUMN_NAME) FOR EXISTING_COL_NAME IN ([Val1], [Val2])
) as Pvt;
</pre>

![result set 1](/assets/img/resultset03.png)

## Conclusion

Official documentation can be found (among other places) at [https://docs.microsoft.com/en-us/sql/t-sql/queries/from-using-pivot-and-unpivot](https://docs.microsoft.com/en-us/sql/t-sql/queries/from-using-pivot-and-unpivot). Happy pivoting!