# Recap of DAX vs. MDX Smackdown


At the Feb 2013 meeting of [Atlanta Microsoft Business Intelligence User Group](http://atlantabi.sqlpass.org), the presentation was titled "DAX vs. MDX Smackdown". It was presented by **Neal Waterstreet** ([@NealWaterstreet](https://twitter.com/NealWaterstreet) | [blog](http://www.nealwaterstreet.com/)) and **Damu Venkatesan** ([@vdamu](https://twitter.com/vdamu)), with some assistance by **Teo Lachev** ( [@tlachev](http://twitter.com/tlachev) | [blog](http://prologika.com/CS/blogs/blog/default.aspx)). The presentation compared some of the features, overlaps, and differences between the two languages. During the course of the presentation, there was additional discussion comparing the BISM Tabular model to the BISM Multidimensional model. Understanding the different models is key in understanding the features of the languages.





Neal started off covering the DAX language. DAX appears to be sort of a superset of Excel functions. Apparently, not all Excel functions are represented, and some functions behave differently. Additional, DAX has some functions not included in the Excel language. DAX seem clear enough, but has the same drawback as Excel: once you start nesting functions inside functions inside functions, readability starts to suffer. There was some confusion in the audience over where DAX would be used. You see DAX mentioned in the same breath as "self-service BI", yet someone pointed out they could not see end-users writing DAX queries.



Teo Lachev stepped in and gave a clear summary of the two different BISM models offered in SSAS 2012: BISM Tabular and BISM Multidimensional. I was particularly appreciative of his opinions as to when to choose or recommend Tabular vs. Multidimensional. In addressing the "self-service" question concerning DAX, Teo outlined a high-level usage scenario, where the BI developer would implement the tabular model, extend it with DAX (like creating MDX calculated measures in Multidimensional), and the end-user self-service would be through Excel PowerPivot or Power View. Both Power Pivot and PowerView allow data discovery through a GUI, without requiring knowledge of the query languages.



Damu then presented his summary of MDX. MDX is a powerful language, but it does present learning curve, even for experienced database developers. Damu and Neal demonstrated a few queries in DAX and MDX to achieve the same result set. One thing I learned is that you can query the BISM Tablular model either MDX or DAX. It seems to me that while you could leverage existing MDX knowledge to query the Tabular model, you might be better off using DAX instead. DAX was built specifically for the Tabular model, so my guess is the language is a better fit conceptually.


## Conclusion

While not the primary purpose of the presentation, I came away with my interest piqued in DAX and the BISM Tabular model. I don't see MDX and the BISM Multidimension model going away anytime soon, but there certainly seems to be a lot of momentum from Microsoft in this technology. As a BI Professional, I consider it well worth the time to learn how to build a BISM Tabular model, and to learn DAX.



I give a “Thumbs up” to both Neal and Damu for presenting on a complex topic and thanks to Prologika for sponsoring the meeting. And a special thanks to Teo Lachev for creating and running the Atlanta Microsoft BI User group. Attending this group has helped advance my career, gain new knowledge and meet new friends. I encourage any data professionals (or aspiring data proferssionals) to attend.


## Speakers:


Damu Venkatesan is a Business Intelligence Consultant with Prologika who has over 20 years of IT experience.  His experience includes architecting solutions in Business Intelligence using SQL Server, SharePoint. Prior to becoming a BI Architect, he has served as an Enterprise Data Architect and Database Administrator in several organizations.  He is the founder and co-leader of Healthcare SQL Virtual Chapter for PASS.



Neal Waterstreet is a Business Intelligence Consultant with Prologika who has more than 15 years of industry experience. He has BBA in Computer Information Systems from Georgia State University. An experienced BI architect, Neal is skilled in the entire BI spectrum, including dimensional modeling, ETL processing using Integration Services, Analysis Services cube design and reporting. He is the founder and co-leader of Healthcare SQL Virtual Chapter for PASS.



Prologika is a Microsoft Gold Partner in Business Intelligence, Prologika is a consulting firm that specializes in BI consulting and training. Our mission is to help organizations make sense of data by applying effectively BI technologies. Visit us at [www.prologika.com](http://www.prologika.com).
 
At the Feb 2013 meeting of Atlanta Microsoft Business Intelligence User Group, the presentation was titled "DAX vs. MDX Smackdown". It was presented by Neal Waterstreet and Damu Venkatesan, with some assistance by Teo Lachev. The presentation compared some of the features, overlaps, and differences between the two languages. During the course of the presentation, there was additional discussion comparing the BISM Tabular model to the BISM Multidimensional model. Understanding the different models is key in understanding the features of the languages.
​


