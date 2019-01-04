---
id: 86
title: The Skinny on Databases
date: 2006-03-08T21:08:20+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=86
permalink: /the-skinny-on-databases/
categories:
  - Development Tools
---
Here is [a great article](http://www.developer.com/db/article.php/3589351) on using databases to build web applications â€“ one of the best that I have read in a while.

This article was especially relevant to me because I recently reviewed several projects where data stored in a configuration table was used to set some variables for a VoiceXML application. Most of the project tables (separate table for each project) had no more than few records to specify preferences used to control the behavior of the application. (For example, if the current date falls between a date range stored in the table, play a special greeting file.) Sometimes the data hadnâ€™t been changed since the project was originally developed many months (sometimes a couple of years) before, but no one had ever thought to ask if the application needed to have a database on the back end in the first place.

There are other options that I have floated in such situations. Since the platform I work on supports the [<data> element](http://www.w3.org/TR/2005/CR-voicexml21-20050613/#sec-data) that is part of the VoiceXML 2.1 specification, I think itâ€™s reasonable to ask whether this data can be stored in an XML file, especially if it doesnâ€™t change all that often. I think using the <data> elementâ€™s functionality to read data from an external XML file would be a pretty good use in a scenario like this. Alternatively, whatâ€™s wrong with a simple [JavaScript array](http://developer.mozilla.org/en/docs/Core_JavaScript_1.5_Reference:Global_Objects:Array) to hold a few configuration values?

Iâ€™m not suggesting that alternatives to databases should be used just because they can be, but if it adds a significant amount of additional complexity to an application why not consider another approach?