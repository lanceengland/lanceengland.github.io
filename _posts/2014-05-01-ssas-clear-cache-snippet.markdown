---
date: 2014-05-01 05:12:01
tags: analysis
title: SSAS Clear Cache snippet
---
# SSAS Clear Cache snippet

So, this code snippet is all over the internet, but sometimes I just want to know where I can find it without asking Uncle Google every time.

This XMLA code snippet is for [clearing the SSAS cache](http://technet.microsoft.com/en-us/library/hh230974.aspx) for performance tuning.

<pre data-enlighter-language="sql">
&lt;ClearCache xmlns='http://schemas.microsoft.com/analysisservices/2003/engine'&gt;
  &lt;Object&gt;
  &lt;DatabaseID&gt;your database ID here&lt;/DatabaseID&gt;
  &lt;/Object&gt;
&lt;/ClearCache&gt;
<pre data-enlighter-language="sql">