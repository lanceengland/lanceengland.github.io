# SSAS Clear Cache snippet


So, this code snippet is all over the internet, but sometimes I just want to know where I can find it without asking Uncle Google every time.

This XMLA code snippet is for [clearing the SSAS cache](http://technet.microsoft.com/en-us/library/hh230974.aspx) for performance tuning.
<pre class="codewrap"><code>&lt;ClearCache xmlns='http://schemas.microsoft.com/analysisservices/2003/engine'&gt;
	&lt;Object&gt;
		&lt;DatabaseID&gt;your database ID here&lt;/DatabaseID&gt;
	&lt;/Object&gt;
&lt;/ClearCache&gt;
</code></pre> 
