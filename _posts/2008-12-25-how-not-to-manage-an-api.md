---
id: 354
title: How NOT to Manage an API
date: 2008-12-25T12:39:17+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=354
permalink: /how-not-to-manage-an-api/
jd_tweet_this:
  - 'yes'
wp_jd_clig:
  - http://cli.gs/u7A9JZ
original_post_id:
  - "354"
twitter_cards_summary_img_size:
  - 'a:7:{i:0;i:200;i:1;i:228;i:2;i:1;i:3;s:24:"width="200" height="228"";s:4:"bits";i:3;s:8:"channels";i:3;s:4:"mime";s:9:"image/gif";}'
categories:
  - Open Source
  - Standards
  - Twitter
---
Recently, I decided to build a small application that interacts with the Twitter API, the <a href="http://cli.gs/api" target="_blank">cli.gs</a> API and Google Calendar to allow me to get advanced notice of any and all Boston Celtics games. The app is actually live and can be <a href="http://twitter.com/celticsgames" target="_blank">followed on Twitter</a>.
  
<img src="http://localhost:8000/wp-content/uploads/2008/12/celtics.gif" alt="Boston Celtics" title="Boston Celtics" style="float:left;margin-top:15px;margin-right:15px;margin-bottom:15px;padding-top:10px;padding-bottom:10px;padding-right:10px;" />
  
It&#8217;s a pretty neat little application that runs on a daily `cron` job and queries a small database to see if there is a Celtics game on the following day. If there is a game the next day, it generates a link to add the game to Google Calendar, shortens the link using the cli.gs API and sends out a Tweet with the game details and Calendar link. Pretty sweet, right?

Unfortunately, almost immediately after going live the Twitter API started to return some nasty HTTP responses and I was unable to update the status of the <a href="http://twitter.com/celticsgames" target="_blank">celticsgames</a> account. Specifically, the Twitter API started to return the <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html" target="_blank">HTTP 417 response code</a>. After doing a little Googling, I found that I was <a href="http://groups.google.com/group/twitter-development-talk/browse_thread/thread/7c67ff1a2407dee7?pli=1" target="_blank">not the only person</a> using the Twitter API to suddenly (and without any warning) run into a problem that broke their application.

The issue stems from the inclusion of the HTTP Expect header in the request that is sent to the Twitter API. I&#8217;m using PHP and cURL to interact with the Twitter API. Apparently, this header is sent by default with a value of &#8216;100-continue&#8217; by cURL (unless you override it and set a different value). Seems the Twitter API grew very confused by this setting in the past few days. The HTTP spec states that:

> A server that does not understand or is unable to comply with any of the expectation values in the Expect field of a request MUST respond with appropriate error status. The server MUST respond with a 417 (Expectation Failed) status if any of the expectations cannot be met or, if there are other problems with the request, some other 4xx status. 

I did some checking and I can&#8217;t find any mention on the Twitter API Wiki about a recent change that would have affected how the Twitter API responded to HTTP requests with the Expect header set in any particular way (it is possible, though, that I could have missed it). I also referred to the specific list of <a href="http://apiwiki.twitter.com/REST-API-Documentation?SearchFor=expect+header&sp=1#HTTPStatusCodes" target="_blank">HTTP status codes</a> returned by the API &#8212; no mention of a 417 response at all.

In my opinion, this is a pretty crappy way to manage an API. If your goal is to encourage third-party developers to build applications that work with your API, don&#8217;t make breaking changes without going to great lengths to ensure the developer community understands how the change will impact them. And for goodness sake, update your documentation.

Fortunately, there is a Celtics game on later today (during which they will absolutely pound the LA Lakers) &#8212; that should take this bad taste out of my mouth.