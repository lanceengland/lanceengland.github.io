---
date : 2014-05-22 11:24:22
tags: analysis
---
# SSAS Default NULL Measure

Today's post is another of the "place to put a code snippet so I can easily find it in the future" variety.

When browsing a cube, end users will often choose their dimensions first before selecting a measure. In cases with a very large dimension or a very large default measure, this can often cause a long delay. One way of addressing this is by defining a default NULL measure. This eliminates the delay in those cases. The only trade-off is that initially (before a measure is explicitly picked) the dimension members won't actually display since the result for every tuple will be NULL. The code snippet below goes in the SSAS script section.

<pre data-enlighter-language="sql">
// default null member
CREATE MEMBER CURRENTCUBE.[Measures].[DefaultNullMeasure] AS NULL,
VISIBLE = 0;
ALTER CUBE CURRENTCUBE UPDATE DIMENSION Measures,
DEFAULT_MEMBER = [Measures].[DefaultNullMeasure];
</pre>

Credit goes to Chris Webb via Jorg Klein for posting about this [five years ago](http://sqlblog.com/blogs/jorg_klein/archive/2009/02/06/ssas-speed-up-dimensions-using-a-null-default-cube-measure.aspx). My post is merely a quicker way for me to reference the code.