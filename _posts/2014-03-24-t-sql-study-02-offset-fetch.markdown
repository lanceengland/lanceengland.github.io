---
date: 2014-03-24
tags: data
title: T-SQL Study&#58; OFFSET FETCH
---
# T-SQL Study: OFFSET FETCH

Continuing the [T-SQL study series](/blog/2014/3/24/t-sql-study-1-create-view-and-procedure-options), the award for "Most Awkward Implementation" goes to... OFFSET FETCH!

To be fair, this is not on Microsoft. This is an ANSI implementation, and for that Microsoft should be commended. It's too bad the standard is a little wordy and klunky. Enough whinging, here's the scoop.

Paging through result sets, the preferred way is now with OFFSET/FETCH:

- It declares our intentions to the database engine, allowing for implementation optimizations.

- Did I mention it's in the ANSI SQL standard?

The ~~klunky~~ syntax:

<pre data-enlighter-language="sql">
OFFSET n ROW[S] FETCH [FIRST\|NEXT} n ROW[S] ONLY
</pre>

Note:

- ROW and ROWS are synonyms (for readability only)

- FIRST and NEXT are also synonyms (same reason)

- FETCH cannot exist without OFFSET

- The whole clause cannot exists with an ORDER BY

- The OFFSET and FIRST/NEXT value can be parameterized. This can be used for pagination. See code sample below.

<pre data-enlighter-language="sql">
DECLARE @ResultsPerPage INT = 10;
DECLARE @PageNumber INT = 0;

SELECT SalesOrderNumber, OrderQuantity, ProductKey  
FROM dbo.FactInternetSales
ORDER BY ORDERDATE
OFFSET (@PageNumber * @ResultsPerPage) ROWS FETCH NEXT @ResultsPerPage ROWS ONLY;
</pre>
