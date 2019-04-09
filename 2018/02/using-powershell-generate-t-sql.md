# Using PowerShell to generate T-SQL


Lazy or inspired? Working harder, or smarter? Hopefully the later in both. I&#39;ve been slinging a lot of T-SQL code lately, and much of it repeated patterns. I realize tools like [BIML](https://varigence.com/Biml) exist for just such a purpose. However, BIML has it&#39;s own learning curve for the scripting part, and my scripting tool of choice lately is PowerShell. So, I decided to make a tool in PowerShell.



MERGE is a SQL command with verbose syntax, and one I am using a lot. It does have some caveats, as documented in Aaron Bertrand&#39;s post [Use Caution with SQL Server&#39;s MERGE Statement](https://www.mssqltips.com/sqlservertip/3074/use-caution-with-sql-servers-merge-statement/). However, with careful use, I find it an invaluable tool.



The result of my efforts is a function called Get-SqlMergeStatement. The repo is found at [https://github.com/lanceengland/SqlHelpers](https://github.com/lanceengland/SqlHelpers).



An example of how to use the function:

<script src="https://gist.github.com/lanceengland/493795b4ee49bbd30988f21d557f639e.js"></script>


Which produces the following output:

<script src="https://gist.github.com/lanceengland/f6c94d25406c928a469a8219e3730bcd.js"></script>


The function is in a plain PS1 file, but if I add another function I&#39;ll make it a module. As an aside, I continue to be impressed with the power and expressiveness of PowerShell. The entire function is a template string, with embedded snippets that iterate over the same string array and apply different transformations through the pipeline.



If this looks like something you can benefit from, great! Feel free to use, comment, or contribute.

 
