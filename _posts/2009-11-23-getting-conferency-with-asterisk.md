---
id: 1385
title: Getting Conferency With Asterisk
date: 2009-11-23T20:10:30+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1385
permalink: /getting-conferency-with-asterisk/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] Getting Conferency With Asterisk. #url# #Asterisk #VoIP'
wp_jd_clig:
  - http://cli.gs/M5qWp
wp_jd_target:
  - http://www.voiceingov.org/blog/?p=1385
original_post_id:
  - "1385"
categories:
  - Asterisk
  - Open Source
---
A good indicator of just how powerful and useful Asterisk can be is demonstrated by the amount of effort that is required to build a feature rich conference call application. What if I told you it can be done in _6 lines of code_?

(Yes, _extensions.conf_ is software &#8211; when you work with it, you are writing software code. See &#8220;When You Write extensions.conf, You Are Writing Software&#8221; by @jicksta [in this post](http://jicksta.com/posts/what-were-not-admitting-about-asterisk) for more on this.)

Create a conference room by editing meetme.conf:

> `$ echo "conf => 1234,2345,5678" >> /etc/asterisk/meetme.conf`

Or you can open the file with your favorite editor and add the new line at the bottom. This will create a conference numbered 1234, with a user PIN of 2345 and an administrator PIN of 5678. Next create a dialplan context and rule set for the conference room

> `[MyFirstConferenceRoom]<br />
exten => 9000,1,Answer()<br />
exten => 9000,n,Meetme(1234,ips)<br />
exten => 9000,n,Hangup()`

The parameters passed into the Meetme() application are the number of our conference room (just created in meetme.conf) and a set of options. The **i** option enables an announcement each time someone enters or leaves the room. The **p** option allows a user to exit the conference by pressing the &#8216;#&#8217; key. The **s** option allows a user (regular or admin) to hear a menu of options when the &#8216;*&#8217; key is pressed.

Now, include the conference room context in your primary dialplan context:

> `include =>  MyFirstConferenceRoom`

That&#8217;s it &#8211; to test it out, make sure you reload your dialplan, and then dial 9000.

You&#8217;ve got to love how easy it is to get conferency with Asterisk!