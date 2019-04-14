---
date : 2017-12-27 09:12:27
tags: data
---
# MERGE Quick Reference (T-SQL)

The T-SQL MERGE statement allows 'upsert' functionality (UPDATE or INSERT) in one statement. It&#39;s usage comes with some caveats, documented in Aaron Bertrand&#39;s post [Use Caution with SQL Server&#39;s MERGE Statement](https://www.mssqltips.com/sqlservertip/3074/use-caution-with-sql-servers-merge-statement/). The syntax is a little difficult to remember, so I am keeping a quick post here for reference.

<pre data-enlighter-language="sql">
MERGE INTO
  SchemaName.TableName WITH (HOLDLOCK) AS TGT
USING
  (VALUES
  (1, 'aaa'),
  2, 'bbb'),
  3, 'ccc'),
  4, 'ddd')
  ) AS SRC(Pk, ColB)
  ON (SRC.Pk = TGT.Pk)

WHEN NOT MATCHED BY TARGET THEN
  INSERT (Pk, ColB)
  VALUES (SRC.Pk, SRC.ColB)

WHEN MATCHED AND EXISTS (SELECT SRC.ColB EXCEPT SELECT TGT.ColB) THEN /* Don't update if the values are the same */
  UPDATE SET
  ColB = SRC.ColB

WHEN NOT MATCHED BY SOURCE THEN DELETE /* Use with caution, but will keep the target table in sync with the source */
;
</pre>