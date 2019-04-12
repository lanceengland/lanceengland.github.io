---
date : 2018-02-06 04:13:06
---
# Using PowerShell to generate T-SQL

Lazy or inspired? Working harder, or smarter? Hopefully the later in both. I've been slinging a lot of T-SQL code lately, and much of it repeated patterns. I realize tools like [BIML](https://varigence.com/Biml) exist for just such a purpose. However, BIML has it's own learning curve for the scripting part, and my scripting tool of choice lately is PowerShell. So, I decided to make a tool in PowerShell.

MERGE is a SQL command with verbose syntax, and one I am using a lot. It does have some caveats, as documented in Aaron Bertrand's post [Use Caution with SQL Server's MERGE Statement](https://www.mssqltips.com/sqlservertip/3074/use-caution-with-sql-servers-merge-statement/). However, with careful use, I find it an invaluable tool.

The result of my efforts is a function called Get-SqlMergeStatement. The repo is found at [https://github.com/lanceengland/SqlHelpers](https://github.com/lanceengland/SqlHelpers).

An example of how to use the function:

<pre data-enlighter-language="shell">
Get-SqlMergeStatement -TargetTableName Tbl -JoinColumns a -MergeColumns a,b,c
</pre>

Which produces the following output:

<pre data-enlighter-language="sql">
WITH SRC AS
( 
    /* your source query here */
)
MERGE INTO Tbl WITH (HOLDLOCK) AS TGT
    USING SRC ON (SRC.a = TGT.a)

WHEN NOT MATCHED BY TARGET THEN
    INSERT (
        a,
        b,
        c
    )
    VALUES (
        SRC.a,
        SRC.b,
        SRC.c
    )

WHEN MATCHED AND EXISTS (
    SELECT SRC.b, SRC.c
    EXCEPT
    SELECT TGT.b, TGT.c
    )
THEN
    UPDATE SET
    b = SRC.b,
    c = SRC.c

;
</pre>

The function is in a plain PS1 file, but if I add another function I'll make it a module. As an aside, I continue to be impressed with the power and expressiveness of PowerShell. The entire function is a template string, with embedded snippets that iterate over the same string array and apply different transformations through the pipeline.

If this looks like something you can benefit from, great! Feel free to use, comment, or contribute.