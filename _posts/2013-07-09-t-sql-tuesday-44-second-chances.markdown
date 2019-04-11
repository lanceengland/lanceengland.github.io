---
date : 2013-07-09 11:47:09
---
# T-SQL Tuesday #44: Second Chances

[![T-SQL Tuesday](/assets/img/TSQL2sDay150x150.jpg)](http://www.sqlballs.com/2013/07/t-sql-tuesday-44-second-chance.html)

Sometimes success can be your worst enemy.

What do I mean by that? Working with technology is to continuously solve problems. Software bugs, hardware glitches, exceptions to business logic, on and on it goes. It's the nature of the industry. Some days, the technology gets the upper hand, and other days you feel like the master of the universe. No technical problem can defeat you! It's those days that can turn around and bite you in the behind if you're not careful. With each success confidence builds, and it starts to build on itself. However, getting over-confident can lead to careless errors and taking shortcuts that you otherwise wouldn't have taken. Like, say, making an untested change in production in the middle of the day? What fool would do something like that? *sheepishly raises hand*

Earlier in my career, I was working for a smaller non-profit company that used a donor management system called [The Raiser's Edge](https://www.blackbaud.com/fundraising-crm/raisers-edge-donor-management). The product ran on top of SQL Server, and we had just recently migrated from an in-house custom Microsoft Access application that was starting to show it's age. The limitations of the custom software caused people to enter data in funny ways that otherwise didn't make sense. So, even after doing a meticulous and successful data migration, we still had some post-migration clean-up to do.

This particular example had to do with donations to a particular fund (which we'll call XYZ), which also happened to be the most common designation. The lack of good reporting tools in the old system made it difficult to compare fund performance year-to-year. So, instead the Finance depatment created a new fund for every fiscal year. So, we had a "XYZ 2002-2003", "XYZ 2003-2004", etc.

In The Raiser's Edge, not only was this unnecessary, but it prevented some built-it fund performance reports from running correctly. In other words, all the donations to XYZ should be attributed to one "XYZ" fund, and the reports can easily show historical performance.

OK, so what is the clever data administrator to do? Well, this particular clever data administrator (me) had spent the time to learn how to use the Raiser's Edge VBA API, which among other things provided an object-relational mapping to the underlying data. After a series of small and successful uses of the API, I had gained confidence to tackle something bigger. So, I decided to use my new skills to programatically load all donations that involved an "XYZ"-variant fund, and reassign said fund to the "one true" XYZ fund, then re-save the donation.

Well, the code I wrote was correct, but I made a few mistakes that day. Let me list them below:

- I didn't really understand SQL Server isolation levels at the time

- I had no idea what kind of locking the API was using

- I decided to make large sweeping changes in the middle of the day

- I kicked off the routine then went to lunch

- I left my cell phone at my desk

Well, I think you know where this is going. The process was very slow, because I was changing hundreds of thousands of donations, one at a time. It essentially locked up the front-end application. My co-worker tried to call me to find out what I had done only to have my phone ring 10 feet away from him. When I got back from lunch the place was in quite a tizzy. I found out quickly what had happened and that I was at fault. I felt terrible. We stopped the process and did a point-in-time restore on the database. Then I went and apologized to everyone, to which everyone was very understanding.

So, lessons learned. Don't get cocky! Don't run things in production without testing and a plan (which includes a rollback plan). Don't do things in the middle of the day. And don't leave your cell phone on your desk during lunch! :)

The important thing, of course, is that mistakes happen. But the only way to benefit from mistakes is to learn from them and prevent yourself from making the same mistake twice.