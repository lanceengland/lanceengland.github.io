---
date : 2015-06-03 03:53:03
---
# Hello Wordpress

This is my first post since moving my blog over to the WordPress platform. I am primarily posting this to verify the old RSS feed URL is redirecting correctly to the new RSS feed. I thought I would jot down a few thoughts on the process.

## Why WordPress

WordPress is the most popular blogging platform period. It has an extensive user base, massive plug-in ecosystem, stable code base, and a wealth of community resources. Almost any feature, bug,  or configuration I can think of has been addressed somewhere on the web.

## Why Self-hosted

I considered paying for an account on WordPress.com. It has a lot of benefits and makes a lot of sense. But, I'm a bit of a tinkerer. I want to be able to 'adjust some knobs' on things, sometimes just for the sake of learning more about it. So while I have NO desire to be a WordPress administrator, I did want a little bit more hands-on with the platform. At the same time, I REALLY wanted to give myself an excuse to regularly interact with Microsoft Azure. So, after a bit of research, I decided to self-host on an Azure Web App.

## Azure Web App

I'm not hosting my own virtual machine and administering IIS and WordPress. That is just not cost or time-effective for a low-volume blog. During my platform research, on Azure I created a web app on the Free tier (yes, it's [really free](http://azure.microsoft.com/en-us/pricing/calculator/)) using a WordPress image from the gallery. I spent a week or so finding a theme, exploring the interface, experimenting with exporting my site from SquareSpace and importing it into WordPress, exploring the plug-in ecosystem, etc. Once I was happy with the results, I switched over to the 'Shared' plan for a whopping ~$9/month.

## Domain name

Once you move an Azure web app off the Free plan, you can map a custom domain name to it. I'm not going to recap the steps for you as there is documentation and blog posts that walk through this step. Needless to say, the only tricky part is validating ownership of the domain. This is done through creating a CNAME to be verified by Azure. As DNS takes time to propagate, this step can take some time.

## SSL

Troy Hunt ([@troyhunt](https://twitter.com/troyhunt)), one of my favorite bloggers and PluralSight authors, has a great post of his website on [using SSL for Azure web sites on the Shared tier](http://www.troyhunt.com/2015/04/how-to-get-your-ssl-for-free-on-shared.html) (they aren't supported against custom domain names. However, SSL is supported for your web site's *.azurewebsites.net sub-domain). I did this step to ensure I could log into my site over SSL using my own domain name.

## Conclusion

So far I am happy with the results. This is nothing earth-shattering; all I did was move my site to Azure. I'm looking forward to other ways I can leverage the ever-growing Azure cloud offerings.