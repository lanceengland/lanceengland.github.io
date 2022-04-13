---
date: 
tags: data analysis
title: "Power BI Lessons Learned"
---
# Loading Data via OData and REST API in Power BI

intro

ODATA
Using Web.Contents and dynamic url and relative path
  the static part MUST return 200
  headers record
  OData no headers options, so auth at data source only
  means two different places to manage PAT (parameter and data source)
  continuation token (handled by adapter)

Using Web.Contents POST
  anonymous access
  pass authentication key (BASE64 the 'Basic ' + username:key)
  headers record

delimited text measures with concatenatex (sp?)
