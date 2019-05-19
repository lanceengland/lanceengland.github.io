---
date: 2013-03-22
title: Considering TypeScript
---
# Considering TypeScript

[Mark Headd](http://twitter.com/mheadd/status/154620713649704961), Chief Data Officer for the City of Philadelphia:
> Of all of the languages that compile into JavaScript, I prefer JavaScript.

If you've been paying any attention at all, you know that JavaScript has surged in popularity the past 10 years. The quirky-yet-powerful language has evolved into the [assembly language of the web.](http://www.hanselman.com/blog/JavaScriptIsAssemblyLanguageForTheWebSematicMarkupIsDeadCleanVsMachinecodedHTML.aspx) During this time, many libraries and tools have been created to help smooth over the language quirks. Microsoft has thrown its hat in the ring with an open-source tool named [TypeScript](http://www.typescriptlang.org/). I have been aware of TypeScript since its introduction in 2012, but it had not captured my attention. The topic of the March 2013 [meeting](http://www.meetup.com/Microsoft-Integration-Architects/events/106504092/) of the Atlanta Microsoft Integration Architects user group was *TypeScript - An Introduction*, presented by **Mitch Gordon**. I though it time for me to give it a longer look. After all, it was developed by [Anders Hejlsberg](http://en.wikipedia.org/wiki/Anders_Hejlsberg), whose name alone gives TypeScript gravitas.

## (Very) Brief Description

TypeScript is an opt-in typed language super set of JavaScript, that compiles to JavaScript. The type-checking occurs at design-time, and can be explicit or inferred. It allows modeling classes, modules, and provides information to the tooling for improved IntelliSense support.

In addition, TypeScript supports type declaration files, and allows for library imports like jQuery, provided you supply a corresponding type declaration file for the library.

## Pros

TypeScript *can* help with subtle bugs related to JavaScript type conversion, and will feel more comfortable to those most experienced with class-based languages (C#, Java, etc). And, the improved tool support can speed up development with more intelligent Intellisense.

## Cons

Its another abstraction layer added to a pile of abstractions. You could argue that it removes a strength of the language -- the ability to use a variable not by what it IS, but by what is DOES. I've heard this coined as "duck-typing", where if it acts like a duck, then treat it like a duck. So, the modern JavaScript style is not to test for a class, but test for the existence of function or property as needed.

## My Opinion

I'm still on the fence. I appreciate Mitch's presentation, and he did a fine job. In truth, presently I am not working with JavaScript, so I'm guessing until I actually try it out my opinion of it will be incomplete. What do you think? I welcome your opinions in the comments below, or [tweet me](http://twitter.com/lanceengland).

## Thanks

Thanks to Mitch for speaking, and to Microsoft for hosting. And thank you to the group organizers, especially Mark Rowe who paid for the pizzas/drinks himself. Which reminds me, **the MIA group is looking for sponsors**. If your company is interested, contact the group organizer's through it's [meetup](http://www.meetup.com/Microsoft-Integration-Architects) webpage.

## Speaker

Mitch Gordon is a Senior Consultant with Magenic Technologies. He has been developing web and smart client applications for over a decade and has been working with .Net since Beta. In working with several companies in many different environments, Mitch has seen a lot of different approaches to solving common problems in web development and had been exposed to many best practices and anti-patterns. He has a passion for teaching and sharing from these experiences.
