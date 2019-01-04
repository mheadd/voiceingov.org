---
id: 935
title: 'It&#8217;s the Little Things'
date: 2009-06-04T09:01:27+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=935
permalink: /its-the-little-things/
jd_tweet_this:
  - 'yes'
wp_jd_clig:
  - http://cli.gs/n7ySAY
original_post_id:
  - "935"
categories:
  - Development Tools
tags:
  - transcirption
  - Twilio
---
OK, I&#8217;ll admit it. I&#8217;ve [said some things](http://www.voiceingov.org/blog/?p=438) about Twilio in the past.

To be honest, initially I viewed Twilio as a low-rent competitor to VoiceXML and the [family of W3C languages](http://www.w3.org/Voice/) designed to support telephony applications. However, although I still harbor some doubts about Twilio, my attitude is changing &#8211; largely based on some of the features that Twilio now offers.

**Voice Transcription:**

Now <a href="http://www.twilio.com/docs/api_reference/TwiML/record#transcribe" target="_blank">this is pretty boss</a>. The Twilio XML Markup language (TwiML for short) not only allows developers to make a recording of what a caller says (pretty basic functionality that&#8217;s been in VoiceXML since 1.0), it also lets you have this recording transcribed (up to 2 minutes of speech). Now _this_ is an exciting feature, and although this is a paid feature it is one that I will be trying out in the near future.

**Geographic Information:**

When a call comes into your app, Twilio <a href="http://www.twilio.com/docs/api_reference/TwiML/twilio_request" target="_blank">attempts to look up geographic data</a> based on the <acronym title="Automatic Number Identification">ANI</acronym> and <acronym title="Dialed Number Identification Service">DNIS</acronym> used. This allows developers to have access to to city, state and zip of the calling and called party. Pretty sweet!

Yes its the little things like these that are making Twilio more attractive, and more likely to be used in an upcoming project. Lets hope some of the big VoiceXML hosting providers stand up and take notice, and start to offer similar features to developers.