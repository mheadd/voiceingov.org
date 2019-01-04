---
id: 718
title: Behold the Power of the Command Line
date: 2009-02-23T13:25:42+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=718
permalink: /behold-the-power-of-the-command-line/
original_post_id:
  - "718"
categories:
  - Development Tools
  - Linux
  - Open Source
---
After finding the unbelievingly cool and useful <a href="http://www.commandlinefu.com" target="_blank">Command-line fu</a> website, I have been consumed with finding and using powerful command line tools. Here are some of the tools I&#8217;ve been playing with recently:

**xmlstarlet** &#8211; <a href="http://xmlstar.sourceforge.net/" target="_blank">xmlstarlet</a> is a powerful tool set for using and manipulating XML from the command line. Anyone that interacts regularly with REST-based APIs should give this a look.

**curl** &#8211; I did a [separate post on curl](http://www.voiceingov.org/blog/?p=691) and explained how to use it to interact with the Twitter API. Things get cool fast when you start to combine these tools by piping output from one command to another. For example, using curl and xmlstarlet, you can get your Tweets from the command line (you can change the count parameter in the call to the Twitter API to get a different number of Tweets back):

> `curl -s -u user:password 'http://twitter.com/statuses/friends_timeline.xml?count=5' | xmlstarlet sel -t -m '//status' -v 'user/screen_name' -o ': ' -v 'text' -n`

**festival** &#8211; why read text when you can <a href="http://www.cstr.ed.ac.uk/projects/festival/" target="_blank">have your computer speak to you</a>? I&#8217;m now getting my daily horoscope via curl, xmlstarlet, festival and cron:

> `curl -s 'http://www.trynt.com/astrology-horoscope-api/v2/?m=2&d=23' | xmlstarlet sel -t -m '//horoscope' -v 'horoscope' | festival --tts` 

When you&#8217;ve got tools like this, the command line is where its at!