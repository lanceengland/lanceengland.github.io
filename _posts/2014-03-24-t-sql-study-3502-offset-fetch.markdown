---
date : 2014-03-24 04:24:24
---
# T-SQL Study #02: OFFSET FETCH

Continuing the [T-SQL study series](/blog/2014/3/24/t-sql-study-1-create-view-and-procedure-options), the award for "Most Awkward Implementation" goes to... OFFSET FETCH!

To be fair, this is not on Microsoft. This is an ANSI implementation, and for that Microsoft should be commended. It's too bad the standard is a little wordy and klunky. Enough whinging, here's the scoop.

Paging through result sets, the preferred way is now with OFFSET/FETCH:

- It declares our intentions to the database engine, allowing for implementation optimizations.

- Did I mention it's in the ANSI SQL standard?

The ~~klunky~~ syntax:

OFFSET n ROW[S] FETCH [FIRST|NEXT} n ROW[S] ONLY

Note:

- ROW and ROWS are synonyms (for readability only)

- FIRST and NEXT are also synonyms (same reason)

- FETCH cannot exist without OFFSET (aww, like a tragic love story)

- The whole clause cannot exists with an ORDER BY (now it's a polygamous love story?)

- The OFFSET and FIRST/NEXT value can be parameterized. That's pretty cool. See code sample below.

DECLARE @ResultsPerPage INT = 10;
DECLARE @PageNumber INT = 0;

SELECT SalesOrderNumber, OrderQuantity, ProductKey  
FROM dbo.FactInternetSales
ORDER BY ORDERDATE
OFFSET (@PageNumber * @ResultsPerPage) ROWS FETCH NEXT @ResultsPerPage ROWS ONLY;

<!-- Initialize EnlighterJS -->
<meta name="EnlighterJS" content="Advanced javascript based syntax highlighting" data-indent="4" data-selector-block="pre" data-selector-inline="code" data-rawcodebutton="true" data-language="sql" />

<pre data-enlighter-language="js">
DECLARE @ResultsPerPage INT = 10;
DECLARE @PageNumber INT = 0;

SELECT SalesOrderNumber, OrderQuantity, ProductKey  
FROM dbo.FactInternetSales
ORDER BY ORDERDATE
OFFSET (@PageNumber * @ResultsPerPage) ROWS FETCH NEXT @ResultsPerPage ROWS ONLY;
</pre>
