---
id: 113
title: Expensive != Secure
date: 2007-05-04T09:54:27+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=113
permalink: /expensive-secure/
categories:
  - General Discussion
---
One of the very first jobs I had after getting my undergraduate degree was working on a political campaign. I learned first hand how tight campaigns can be with money â€“ I was paid next to nothing for countless hours of work. Lack of dough notwithstanding, I learned a lot and it opened up a number of opportunities for me down the road.

This first experience was a few years before websites started to be used in political campaigns (hope I didn&#8217;t date myself there). These days, having a campaign website is a requirement. Still, I&#8217;m always amazed at how campaigns choose to invest â€“ in terms of both money and time â€“ in their websites.

There are plenty of ways to stand up a quality web site with a small outlay of actual dollars. Clearly the prorogation of open source software has helped in this regard. I currently volunteer for a campaign that has built <a href="http://www.johncarney.org" target="_blank">it&#8217;s website</a> on the <acronym title="Linux, Apache, MySQL, PHP">LAMP stack</acronym>. We use the phenomenally useful PHP blogging platform <a href="http://wordpress.org/" target="_blank">WordPress</a> for our site. WordPress comes with an active community that contributes a wealth of different <a href="http://wordpress.org/extend/plugins/" target="_blank">plugins</a> that are available to do almost anything a campaign (or any other kind of endeavor) could need.

But investment of dollars is only part of the equation. Anyone that works on a campaign website needs to be aware of <a href="http://www.cnn.com/2006/POLITICS/08/08/lieberman.website/" target="_blank">security issues</a> (at least the obvious ones), and must take steps to mitigate risks. This is the part that is so often missed, even by campings with deep pockets.

I had occasion recently to peruse the website of an elected official rumored to be interested in the same job my candidate is running for. Within about 2 minutes of looking at the site I happened upon a gaping security hole that looked like it was exposing sensitive information for the official in question, and a bunch of other campaigns. I have no idea how long the hole existed, or if anyone else happened upon it.

Based on what I saw, the issue in question could probably be fixed with a one line change to the servers&#8217; Apache configuration file. The server with the gaping hole is maintained by a third-party company that claims to be a leader in supporting campaigns through the use of technology. The lesson here â€“ ask your consultants about security, and if needed get an objective outside opinion. This doesn&#8217;t cost much (if anything) but does require some time and attention.

Fortunately for the official in question, my candidate likes a fair fight. Our campaign contacted the other candidate with information on the security hole, and suggestions for how to work with their consultant to get it fixed. Hopefully, they&#8217;ll do this soon.

I&#8217;d like to think this will get us some good karma down the road in the event that the campaign gets ugly â€“ we&#8217;ll see&#8230;