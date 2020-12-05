---
date: 2020-11-27
title: "DA-100 Study Notes"
---
# DA-100 Study Notes

These are my study notes for [Exam DA-100: Analyzing Data with Microsoft Power BI](https://docs.microsoft.com/en-us/learn/certifications/exams/da-100) upon passing achieves [Microsoft Certified: Data Analyst Associate](https://docs.microsoft.com/en-us/learn/certifications/data-analyst-associate). These notes were all compiled from the offical learning paths and related Microsoft documentation. This is NOT an exam dump.

## Get started with Microsoft data analytics - [2 Modules](https://docs.microsoft.com/en-us/learn/paths/data-analytics-microsoft/)

This learning path summarizes the types of analytics, related roles, and parts of the Power BI platform.

### I. Discover data analysis

Analytics categories:

- Descriptive - what
- Diagnostic - why
- Predictive - trends, eg neural networks, decision trees, regression
- Prescriptive - how to hit target, eg machine learning on past data sets
- Cognitive - Draw inferences from data, and add the findings back to knowledge base for self-learning loop

Roles:

- Business analyst - closer to the business
- Data analyst - reporting specialist, manage reports, data sets, dashboards, etc. The exam is on Data Analyst! The DA tasks are the objectives of the exam
- Data engineer - data flow and integration
- Data scientist - extract value from data through analytics
- Database administrator - operational aspect of data platform

### II. Get started building with Power BI

Power BI is:

- Power BI Desktop
- Power BI service
- Power BI mobile

Building blocks in Power BI:

Visualizations - visual representation of data
Datasets - collection of data
Reports - collection of visualizations on one or more pages
Dashboards - collection of visualizations form one page
Tiles - single visualization

App - a collection of preset visuals, data, reports packaged to share

## Prepare data for analysis - [2 modules](https://docs.microsoft.com/en-us/learn/paths/prepare-data-power-bi/)

This learning path how to connect to various data sources and transform it.

### I. Get data in Power BI

A variety of data source connectors built-in e.g. flat file, relation, NoSql, multi-dimensional, on-line

Storage modes:

- Import
- DirectQuery
- Dual (Composite)

Additional option for connecting to multidimensional i.e. Analysis Services is called Connect Live (formerly named Live Connect).

All queries go through Power Query. Power Query can optimize source queries in some circumstances. The process is called query folding. Any transformation that has an anagalous SQL stement e.g WHERE, GROUP BY, SORT, UNION ALL, and JOIN can generally be folded.

Query Diagnostics is a tool new to me that is part of Power Query. Enable diagnostice, refresh the source(s), then stop the diagnostics. It presents information for each transformation step!

Optimization techniques:

- Process as much at source as possible
- For Direct Query, use native SQL and not stored procs or CTEs
- For Import from relational, don't use embedded SQL as Power Query will NOT perform query folding
- Split date and time columns (this is really a data modeling optimization) as it increase compression (smaller memory footprint).

Power Query also has tools to view data quality, column distribution, and column profile.

Power Query examines the first 1000 rows by default. Change this by clicking the profiling status in the status bar and select 'Column profiling based on entire data set'.

### II. Clean, transform, and load data in Power BI

Append queries are like SQL UNION ALL.

Merge queries are like SQL JOIN (INNER, LEFT OUTER, FULL OUTER)

## Model data in Power BI - [3 modules](https://docs.microsoft.com/en-us/learn/paths/model-power-bi/)

### I. Design a data model in Power BI

Date Table - can be imported from a DW (best), built in Power Query (great), built with DAX (good).

Power Query - =List.Dates(#date(2011,05,31), 365*10, #duration(1,0,0,0)) Then convert the List to Table, then add columns as needed. There are many built-in date conversions in the UI. Highlight the date column, Add Column -> Date (in the From Date & Time group).

DAX - CALENDAR or CALENDARAUTO, then add each column e.g. DayOfMonth, Year, etc. The table won't compress quite as well, but date tables are small enough to have negligible difference.

Mark Table as Date Table. Dates must not duplicate, and not have gaps.

Best Practice - Turn off Auto DateTime, as it builds a hidden date table for each datetime column in the entire data model.

Composite models allow a combination of imported tables and DirectQuery tables. Relationships between imported and DirectQuery tables default to many-to-many, but can be changed. It defauls to many-to-many because it can't detect nor assume uniqueness for the DirectQuery table.

Aggregations are a powerful feature for combining DirectQuery detail data with the performance of imported data. Aggregations import some higher grain of the detail data. A key supporting feature is Dual mode for related dimension tables. Dual mode imports the data for DAX queries against the aggregation, and treating the table as DirectQuery when relating to the detail data. This is a very important performance feature to understand.

More reading:

- [Manage Aggregatons](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-aggregations)
- [Use Composite Models](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-aggregations)
- [Manage Storage Mode](https://docs.microsoft.com/en-us/power-bi/transform-model/desktop-storage-mode)

Data models support both hierarchis, display folders, and table/column hiding to better organize the columns used in analysis. Good model design = good UI design.

### II. Introduction to creating measures using DAX in Power BI

Calculated columns are materialized at data refresh. They do not compress as well as imported columns, meaning they consume more memory.

A simple formula to demonstrate non-additive measure pattern:

<pre data-enlighter-language="sql">
Last Inventory Count =
CALCULATE (
    SUM ( 'Warehouse'[Inventory Count] ),
    LASTDATE ( 'Date'[Date] ))

</pre>

Every time the measure is evaluated, it uses the latest date visible in the filter context.

Tables that only have measures visible (all columns hidden) are displayed at the top of the fields list with a different icon.

### III. Optimize a model for performance in Power BI

## Visualize data in Power BI - 4 modules
### I. Work with Power BI visuals
### II. Create a data-driven story with Power BI reports
### III. Create daskboards in Power BI
### IV. Create paginated reports

## Data analysis in Power BI - 2 modules
### I. Perform analytics in Power BI
### II. Work with AI visuals in Power BI

{% comment %}
{% endcomment %}
