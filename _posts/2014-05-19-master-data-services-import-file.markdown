---
date : 2014-05-19 02:44:19
---
# Master Data Services Import File

I've been working with Microsoft [Master Data Services](http://technet.microsoft.com/en-us/library/ee633763(v=sql.110).aspx) lately and came across [this](http://technet.microsoft.com/en-us/library/ee633854(v=sql.110).aspx) quirky import file specification.

The quirky part is in the description section for replacing existing values with NULL. It's not uncommon for an import file to use a special character string to indicate NULL, but these are especially (ahem) 'special'.

- To change a string attribute value to NULL, set it **~NULL~**.

- To change a number attribute value to NULL, set it to **-98765432101234567890**.

- To change a datetime attribute value to NULL, set it to **5555-11-22T12:34:56**.

So, I can live with the ~NULL~ for the string attribute. But for the number attribute you use a value that is outside the range of BIGINT? And the number selection pattern is the programmer just counting down from 9 to 0 and back up again. Huh? And for the datetime attribute, maybe I'm not smart enough to recognize the significance of November 22, 5555 at 12:34 and 56 seconds, but to me it looks like the programmer just decided...'Hmmm, how about 5555 11 22 123456'.

Is it functional? Yes. Does it look shoddy? Yes.