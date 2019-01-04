---
id: 889
title: VoiceGlue 0.10 Released
date: 2009-06-01T08:34:33+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=889
permalink: /voiceglue-010-released/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - 'VoiceGlue 0.10 Released. #Asterisk + OpenVXI = open source goodness! #opensource #voip #ubuntu'
wp_jd_clig:
  - http://cli.gs/rD9U8A
original_post_id:
  - "889"
categories:
  - Asterisk
  - Open Source
---
For those that don’t know, <a href="http://www.voiceglue.org/" target="_blank">Voiceglue</a> is an open source project that links <a href="http://www.asterisk.org/" target="_blank">Asterisk</a> (the open source PBX) with <a href="http://www.speech.cs.cmu.edu/openvxi/index.html" target="_blank">OpenVXI</a> (an open source VoiceXML platform currently under the stewardship of Vocalocity). Voiceglue makes it possible for Asterisk users to deploy a completely open source VoiceXML platform for building IVRs and other useful applications.

The Voiceglue project recently announced the release of version 0.10 &#8211; there are several new features in this release:

  * Improved audio caching
  * Cookie passing on audio fetching
  * Handles maxage and audiomaxage of 0 properly
  * Uses HTTP Content-Type for audio content when available
  * Defaults to not requiring access-control directive in returned data from data tag
  * New transfer method, new config file param &#8220;blind\_xfer\_method&#8221;
  * Auto-install support for Ubuntu 9.04 (Jaunty)

I&#8217;m especially interested in the last item &#8211; I&#8217;ve been meaning to set up a VM to play around with Ubuntu 9.04 for a few weeks now, and this is yet another good reason for doing so.

The new version of Voiceglue can be <a href="http://www.voiceglue.org/download/" target="_blank">downloaded here</a>.