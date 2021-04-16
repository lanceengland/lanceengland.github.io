---
date: 
tags: data analysis
title: "Power BI Lessons Learned"
---
# Power BI Lessons Learned

intro

braindump

- Web.Contents and dynamic url and relative path
- Web.Contents POST
  - anonymous access
  - pass authentication key (BASE64 and the = sign)
- OData no headers options, so auth at data source only
- means two different places to manage PAT (parameter and data source)
- also means configuring PAT at dataflow (though data source looks to be at the workspace level)
- promoting dataflows requires parameterizing GUIDs for both workspace and dataflow (bleech)
- joins/merges in PQ are slow for large tables. can speed things up by adding an index on the smaller table (or right table). Can do this manually Table.AddKey or using 'Remove Duplicates'
- OData.Feed vs Web.Contents (nextLink)
- HTML to text function
- converting a list to CSV
- maybe how to do continuation tokens with Web.Contents and List.Generate
- using data flows; strentghs and limits for pro
- round peg and square hole ... hello data model (bidierctional)
- using postman with pat with devops (probably omit)
- csv text measures
- limitations of direct query (odbc, custom requires gateway)
- limitations of python script in (personal gateway only, script runs on gateway machine)
