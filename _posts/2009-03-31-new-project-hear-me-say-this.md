---
id: 759
title: 'New Project: Hear Me Say This'
date: 2009-03-31T11:29:05+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=759
permalink: /new-project-hear-me-say-this/
jd_tweet_this:
  - 'yes'
wp_jd_clig:
  - http://cli.gs/YaJ7DR
original_post_id:
  - "759"
categories:
  - Cell Phones
  - Open Source
  - Twitter
---
I haven&#8217;t been blogging much of late because of a new project I have been working on &#8212; _<a href="http://hearmesaythis.org/" target="_blank">Hear Me Say This</a>_.

_<a href="http://hearmesaythis.org/" target="_blank">Hear Me Say This</a>_ is a web application that uses the Sunlight Labs API, the Twitter API and the plain old telephone to empower citizens to send a message to the people that represent them in Congress.

The application uses the <a href="http://www.voxeo.com/free/" target="_blank">Voxeo Prophecy platform</a> to support the VoiceXML and CCXML components it uses, and is hosted on a lean and mean Ubuntu web server. The back end is written in PHP 5 and uses a MySQL database. Its open source, and uses the latest open standards for developing telephone applications.

I&#8217;ve entered this application in the Sunlight Labs &#8220;<a href="http://www.sunlightlabs.com/appsforamerica/" target="_blank">Apps for America</a>&#8221; contest &#8212; I&#8217;ve been pleasantly surprised by the number of telephone apps entered in this contest (the others all appear to be Asterisk-based, mine is the only CCXML/VoiceXML-based app).

Now that the project is done, and my entry has been submitted, I&#8217;m focused on adding features and spreading the word. If you&#8217;re interested in testing it out, [give me a shout](mailto:mheadd@voiceingov.org).

One of the other things I&#8217;d like to do in the weeks ahead is to convert the voice portion of this application to use Voxeo&#8217;s new <a href="http://www.tropo.com/" target="_blank">Tropo</a> platform. Since the back end of the app is written in PHP, and since Tropo supports writing voice apps in PHP I think it would be an interesting experiment to rewrite the existing CCXML/VoiceXML pieces as PHP/Tropo components.

I think this will provide a nice way of comparing and contrasting voice applications built using the more (ahem) traditional VoiceXML route vs. the new hotness that is Tropo.

Stay tuned!