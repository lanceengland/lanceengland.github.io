---
date: 2021-06-05
tags: analysis data
title: "Viewing Column Sizes in Power BI Desktop"
---
# Viewing Column Sizes in Power BI Desktop

When performance tuning Power BI data models, the most important consideration is the size of each column. The tabular model is a columnar database which means the data values are organized by column, allowing for increased compression. Analytic workloads, such as Power BI, benefit greatly from this.

Because the entire data model is loaded in memory, the smaller the data model, the greater the performance gain. The first step in performance tuning is always measurement, and the first measurement is the size of each column. Luckily there is an easy way to do this using [DAX Studio](https://daxstudio.org/) and the [DISCOVER_STORAGE_TABLE_COLUMNS](https://docs.microsoft.com/en-us/openspecs/sql_server_protocols/ms-ssas/1079969e-b432-48fc-bff7-20ced3a96893) dynamic management view (DMV).

Open DAX Studio, connect to your Power BI Desktop data model. Behind the scenes, the data model is a SQL Server Analysis Server Tabular Model, so it supports a host of dynamic management views. The query below lists column name, table name, and column size in descending order. Once you identify the largest columns, you can decide if and how to deal with it. 

Your optimization options are:

1. Delete it if you don’t need it
2. Split it (for example split dates and times into two columns, splitting order numbers if they follow a pattern that reuses same values that would benefit from column compression)
3. Limit the number of distinct values (meaning group outliers into a catch-all bucket)
4. Do nothing, because you need the column

The query:

<pre data-enlighter-language="sql">
SELECT
    DIMENSION_NAME,
    ATTRIBUTE_NAME,
    DICTIONARY_SIZE
FROM
    $System.DISCOVER_STORAGE_TABLE_COLUMNS
WHERE
    COLUMN_TYPE = 'BASIC_DATA'
ORDER BY
    DICTIONARY_SIZE DESC
</pre>

![DMV query and results](/assets/img/dax_studio_dmv.png)
