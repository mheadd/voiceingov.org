---
id: 134
title: Government Technology Mistakes
date: 2008-04-16T13:55:22+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=134
permalink: /government-technology-mistakes/
original_post_id:
  - "134"
categories:
  - General Discussion
---
One of the things that makes government agencies unique is that they are repositories for vast amounts of information &#8212; some of which is highly sensitive (i.e., tax data), or that could be damaging if it fell into the wrong hands (i.e., social security numbers).

Accounts of government agencies exposing sensitive data, like social security numbers, are [shockingly common](http://www.nytimes.com/2007/04/21/washington/21data.html). Many of these stories share an underlying theme or two. One of the most frustrating &#8212; poor web programming.

The State of Oklahoma&#8217;s Department of Corrections is the [latest to join](http://thedailywtf.com/Articles/Oklahoma-Leaks-Tens-of-Thousands-of-Social-Security-Numbers,-Other-Sensitive-Data.aspx) this embarrassing fraternity of government entities that have completely overlooked the most basic tenets of good web development and exposed sensitive information.

> _One of the cardinal rules of computer programming is to never trust your input. This holds especially true when your input comes from users, and even more so when it comes from the anonymous, general public. Apparently, the developers at Oklahoma's Department of Corrections slept through that day in computer science class, and even managed to skip all of Common Sense 101. You see, not only did they trust anonymous user input on their public-facing website, but they blindly executed it and displayed whatever came back.</p> 
> 
> The result of this negligently bad coding has some rather serious consequences: the names, addresses, and social security numbers of tens of thousands of Oklahoma residents were made available to the general public for a period of at least three years.</em></blockquote> 
> 
> Including an SQL statement as a URL parameter and then blindly allowing your web application to execute said parameter is a recipe for disaster.
> 
> After reading this, I thought it might be interesting to see how common this problem might be with other government website. I decided to run a quick check using one of the advanced search features of Google to look for SQL keywords in web site URLs that have &#8220;.gov&#8221; in them. The results? Very, very <a href="http://www.google.com/search?hl=en&q=allinurl%3ASELECT+FROM+WHERE+.gov&btnG=Search" target="_blank">Depressing</a>.
> 
> The fact that the Oklahoma DOC allowed the social security numbers of individuals to be displayed through SQL manipulation masks a deeper issue &#8212; why are these social security numbers being stored in unencrypted form in their database? If there isn&#8217;t a legitimate reason to store this data in an unencrypted form, then it should be avoided at all costs. (And the database that holds the unencrypted version should be nowhere near a web server.) [One way encryption](http://www.webmonkey.com/webmonkey/00/20/index4a.html) works just fine for most authentication needs.
> 
> This same issue was the cause of a breach at the [University of Delaware](http://www.udel.edu/PR/UDaily/2006/mar/classifieds031006.html) a few years back &#8212; wonder if they learned their lesson?