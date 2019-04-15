---
date : 2014-07-24 04:01:24
tags: data analysis
---
# Extending the AdventureWorksDW2012 Date Dimension table

Yesterday on Twitter I came across a link to Dustin Ryan's blog post [Script To Populate AdventureWorksDW DimDate](http://www.bidn.com/blogs/DustinRyan/bidn-blog/6223/script-to-populate-adventureworksdw-dimdate).

Dustin did a great job with his script. I had considered doing the same in the past, but never followed through. Being a developer, I couldn't help but tinker with his work. ;) I have posted my own version below. Dustin did 90% of the work; I made the following changes:

- Used a set-based approach using a CTE numbers table (courtesy of [Itzik Ben-Gan](http://sqlmag.com/sql-server/virtual-auxiliary-table-numbers))

- Used a MERGE statement

- Tweaked all the DATEADD and DATEPART functions to use the full datepart name instead of the abbreviation for better readability

- Even managed to work in a [parameterized OFFSET/FETCH](/blog/2014/3/24/t-sql-study-3502-offset-fetch) statement

At some point I want to create a script to build out the fact tables for years 2008 through current year. Maybe I'll wait for someone to do 90% of the work first :)

<pre data-enlighter-language="sql">
declare @StartDate date = '20050101';
declare @EndDate date = '20151231';
declare @totalDays int = DateDiff(day, @StartDate, @EndDate);

WITH
L0   AS(SELECT 1 AS c UNION ALL SELECT 1),
L1   AS(SELECT 1 AS c FROM L0 AS A CROSS JOIN L0 AS B),
L2   AS(SELECT 1 AS c FROM L1 AS A CROSS JOIN L1 AS B),
L3   AS(SELECT 1 AS c FROM L2 AS A CROSS JOIN L2 AS B),
L4   AS(SELECT 1 AS c FROM L3 AS A CROSS JOIN L3 AS B),
Nums AS(SELECT ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS n FROM L4)
merge into dbo.DimDate with (holdlock) as TGT
using (
select
convert(int,convert(varchar(10),DateAdd(day, n, @StartDate),112)) as DateKey,
DateAdd(day, n, @StartDate) as FullDateAlternateKey,
datepart(weekday,DateAdd(day, n, @StartDate)) as DayNumberOfWeek,
datename(weekday,DateAdd(day, n, @StartDate)) as EnglishDayNameOfWeek,
(Select top 1 SpanishDayNameOfWeek From DimDate Where EnglishDayNameOfWeek = datename(weekday,DateAdd(day, n, @StartDate))) as SpanishDayNameOfWeek,
(Select top 1 FrenchDayNameOfWeek From DimDate Where EnglishDayNameOfWeek = datename(weekday,DateAdd(day, n, @StartDate))) as FrenchDayNameOfWeek,
datepart(day,DateAdd(day, n, @StartDate)) as DayNumberOfMonth,
datepart(dayofyear,DateAdd(day, n, @StartDate)) as DayNumberOfYear,
datepart(week, DateAdd(day, n, @StartDate)) as WeekNumberOfYear,
datename(MONTH,DateAdd(day, n, @StartDate)) as EnglishMonthName,
(Select top 1 SpanishMonthName From DimDate Where EnglishMonthName = datename(MONTH,DateAdd(day, n, @StartDate))) as SpanishMonthName,
(Select top 1 FrenchMonthName From DimDate Where EnglishMonthName = datename(MONTH,DateAdd(day, n, @StartDate))) as FrenchMonthName,
Month(DateAdd(day, n, @StartDate)) as MonthNumberOfYear,
datepart(quarter, DateAdd(day, n, @StartDate)) as CalendarQuarter,
year(DateAdd(day, n, @StartDate)) as CalendarYear,
case datepart(quarter, DateAdd(day, n, @StartDate))
    when 1 then 1
    when 2 then 1
    when 3 then 2
    when 4 then 2
end as CalendarSemester,
case datepart(quarter, DateAdd(day, n, @StartDate))
    when 1 then 3
    when 2 then 4
    when 3 then 1
    when 4 then 2
end as FiscalQuarter,
case datepart(quarter, DateAdd(day, n, @StartDate))
    when 1 then year(DateAdd(day, n, @StartDate))
    when 2 then year(DateAdd(day, n, @StartDate))
    when 3 then year(DateAdd(day, n, @StartDate)) + 1
    when 4 then year(DateAdd(day, n, @StartDate)) + 1
end as FiscalYear,
case datepart(quarter, DateAdd(day, n, @StartDate))
    when 1 then 2
    when 2 then 2
    when 3 then 1
    when 4 then 1
end as FiscalSemester

from Nums
order by n
offset 0 rows fetch first @totalDays rows only
) as SRC
ON (TGT.DateKey = SRC.DateKey)
WHEN NOT MATCHED THEN
INSERT (DateKey, FullDateAlternateKey, DayNumberOfWeek, EnglishDayNameOfWeek, SpanishDayNameOfWeek, FrenchDayNameOfWeek, DayNumberOfMonth, DayNumberOfYear, WeekNumberOfYear, EnglishMonthName, SpanishMonthName, FrenchMonthName, MonthNumberOfYear, CalendarQuarter, CalendarYear, CalendarSemester, FiscalQuarter, FiscalYear, FiscalSemester)
VALUES (SRC.DateKey, SRC.FullDateAlternateKey, SRC.DayNumberOfWeek, SRC.EnglishDayNameOfWeek, SRC.SpanishDayNameOfWeek, SRC.FrenchDayNameOfWeek, SRC.DayNumberOfMonth, SRC.DayNumberOfYear, SRC.WeekNumberOfYear, SRC.EnglishMonthName, SRC.SpanishMonthName, SRC.FrenchMonthName, SRC.MonthNumberOfYear, SRC.CalendarQuarter, SRC.CalendarYear, SRC.CalendarSemester, SRC.FiscalQuarter, SRC.FiscalYear, SRC.FiscalSemester)
;
</pre>