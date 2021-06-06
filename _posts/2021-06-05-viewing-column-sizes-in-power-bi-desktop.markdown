---
date: 2021-06-05
tags: analysis
title: "Viewing Column Sizes in Power BI Desktop"
---
# Viewing Column Sizes in Power BI Desktop

When performance tuning Power BI data models, the most important consideration is column size. The tabular model is a columnar database which means the data values are organized by column. This allows for increased compression due to typically many repeated values in the same column. Analytic workloads, such as Power BI, benefit greatly from this.

Because the entire data model is loaded in memory, the smaller the data model the greater the performance gain. The first step in performance tuning is always measurements. And the first thing to measure is the size of your columns in memory. Luckily there is an easy way to do this.

Using [DAX Studio](https://daxstudio.org/), connect to your Power BI Desktop data model. Behind the scenes, the data model is a SQL Server Analysis Server Tabular Model, so it supports a host of dynamic management views. The query below lists column name, table name, and column size in descending order. Once you identify the largest columns, you can decide if and how to deal with it. Options are usually:

1. Delete it if you don’t need it
2. Split it (for example split dates and times into two columns, splitting order numbers if they follow a pattern that reuses same values that would benefit from column compression)
3. Do nothing

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
