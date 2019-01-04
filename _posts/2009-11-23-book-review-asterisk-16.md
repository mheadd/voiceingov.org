---
id: 1380
title: 'Book Review: Asterisk 1.6'
date: 2009-11-23T10:10:56+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1380
permalink: /book-review-asterisk-16/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "1380"
categories:
  - Asterisk
tags:
  - AGI
  - dial plan
  - VoIP
---
The folks at Packt Publishing recently sent me a copy of **<a href="http://www.packtpub.com/build-feature-rich-telephony-system-with-asterisk-1-6/mid/230909ak97vd?utm_source=It-Slav.net&utm_medium=affiliate&utm_content=other&utm_campaign=mdb_000777" target="_blank">Asterisk 1.6</a>** by David Merel, Barrie Dempster and David Gomillion and I&#8217;ve been using it for the past week or so to set up a fresh instance of Asterisk 1.6 on Ubuntu 8.04.

(Note &#8211; like many Asterisk books, this one is _very_ CentOS focused, but I found the installation and configuration steps described in it &#8211; as well as some of the larger Asterisk management concepts &#8211; easily applied to Debian-based distros like Ubuntu.)

This is a solid, well-written book for anyone that wants to start building and managing an Asterisk-based telephony system. I found this book to be very well focused on concepts that would appeal to someone who wants to manage an Asterisk system professionally, and is probably less well suited for someone interested in tinkering with Asterisk as a hobby.

There is a good discussion of some of the key concepts that someone who aspires to be (or already is) an Asterisk professional should have a handle on. It has a very good discussion on hardening an Asterisk server in the chapter on &#8220;Maintenance and Security&#8221;, and the book begins (very appropriately) with an exercise in developing a deployment plan &#8211; again, probably not well suited for an Asterisk or VoIP tinkerer, but critical for someone who is going to deploy an Asterisk server in a production environment and assume responsibility for managing it.

I didn&#8217;t see any discussion of some of the more cutting edge features of Asterisk 1.6 that might be of interest to someone wanting in exploring alternative communication protocols (Jabber/Jingle) or some of the less mainstream channel drivers (GTalk, Skype, etc.) There is no mention at all (that I could see) of how to connect Asterisk to Google Talk via Jingle, or to the Skype network via <a href="http://blogs.digium.com/2009/02/23/skype-for-asterisk-update/" target="_blank">Skype for Asterisk</a>. Nor is there any mention of some of the IM-focused dialplan applications like <a href="http://www.voip-info.org/wiki/view/Asterisk+cmd+JabberSend" target="_balnk">JabberSend()</a>.

This book appears to be designed for someone who wants to set up and manage a more traditional Asterisk-based system. And for that purpose it is very well suited.