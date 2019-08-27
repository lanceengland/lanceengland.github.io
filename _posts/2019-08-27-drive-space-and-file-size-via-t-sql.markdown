---
date: 2019-08-27
tags: data
title: "Drive Space and File Size via T-SQL"
---
# Drive Space and File Size via T-SQL

I keep having to use these scripts to I figured I'd put them here for convienience.

Get drive space for drives on the SQL Server

<pre data-enlighter-language="sql">
EXEC MASTER..xp_fixeddrives;
</pre>

Get file sizes for all databases

<pre data-enlighter-language="sql">
SELECT
    DB_NAME(database_id) AS [Database],
    Name,
    Physical_Name,
    ROUND((CAST(size AS float) * 8)/1024/1024, 2) AS SizeGB
FROM
    sys.master_files
ORDER BY
    SizeGB DESC
;
</pre>