# T-SQL Tuesday #104: Code I would hate to live without


This month's T-SQL Tuesday is hosted by Bert Wagner [[t](https://bertwagner.com/) | [b](https://twitter.com/bertwagner)]. This month’s topic is 'What code would you hate to live without?'


<a href="https://bertwagner.com/2018/07/03/code-youd-hate-to-live-without-t-sql-tuesday-104-invitation/"><img src="https://lance-england.com/wp-content/uploads/TSQL2sDay150x150.jpg" alt="T-SQL Tuesday #105 hosted by Bert Wagner" width="150" height="150" class="alignnone size-full wp-image-901" style="float:left" /></a>


My entry is a tiny bit of a recycle from a previous post, but, since no one reads my blog anyway I don't expect much protest. :)  In my current role, I have been writing a **lot** of SQL, particularly ETL-type code where data lands in a staging table then needs to be inserted or updated ala 'upserted' into the final destination. I've been using the T-SQL MERGE statement, which is one of the more verbose commands, especially when a lot of columns are involved. Being <del datetime="2018-07-10T22:22:21+00:00">lazy </del> efficient-minded, I decided to write a little tool to generate a full MERGE command by passing in a table name and column names. My scripting tool of choice lately is PowerShell. So, I decided to make a tool in PowerShell and post it on GitHub.



The result of my efforts is a function called Get-SqlMergeStatement. The repo is found at [https://github.com/lanceengland/SqlHelpers](https://github.com/lanceengland/SqlHelpers).



An example of how to use the function:

<script src="https://gist.github.com/lanceengland/493795b4ee49bbd30988f21d557f639e.js"></script>


Which produces the following output:

<script src="https://gist.github.com/lanceengland/f6c94d25406c928a469a8219e3730bcd.js"></script>


The code is a plain PS1 file. At some point I might try to add a PIVOT and UNPIVOT function, as the syntax for those functions always bends my brain a bit. The entire function is a template string, with embedded snippets that iterate over the same string array and apply different transformations through the pipeline.



Anyway, I'm sure I **could** live without this code, but it would just take a lot longer to write MERGE statements. If this looks like something you can benefit from, great! Feel free to use, comment, or contribute. Thanks Bert for hosting!

 
