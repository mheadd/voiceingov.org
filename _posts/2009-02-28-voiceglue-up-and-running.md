---
id: 737
title: VoiceGlue Up And Running
date: 2009-02-28T11:24:45+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=737
permalink: /voiceglue-up-and-running/
jd_tweet_this:
  - 'yes'
wp_jd_clig:
  - http://cli.gs/XPygap
original_post_id:
  - "737"
categories:
  - Asterisk
  - Development Tools
  - Linux
---
I now have <a href="http://www.voiceglue.org/" target="_blank">VoiceGlue</a> up and running on Ubuntu 8.10. (Actually, the Ubuntu server is running as a virtual machine under Sun&#8217;s VirtualBox 2.1.)

For those that don&#8217;t know, <a href="http://www.voiceglue.org/" target="_blank">VoiceGlue</a> is an open source project that links <a href="http://www.asterisk.org/" target="_blank">Asterisk</a> (the open source PBX) with <a href="http://www.speech.cs.cmu.edu/openvxi/index.html" target="_blank">OpenVXI</a> (an open source VoiceXML platform currently under the stewardship of Vocalocity). VoiceGlue makes it possible for Asterisk users to deploy a completely open source VoiceXML platform for building IVRs and other useful applications.

The VoiceGlue install on Ubuntu 8.10 went smoothly &#8212; I did [run into an issue](http://www.voiceingov.org/blog/?p=730) with one of the services not starting, but that was easily identified and fixed thanks to a speedy response from the VoiceGlue folks. (This issue was really my own fault &#8212; use the `pgrep` command to make sure you have specific services running. And when in doubt, check the logs people!)

Based on my experience with the install and my initial testing I am extremely impressed with VoiceGlue. Its <a href="http://voiceglue.org/wiki/doku.php" target="_blank">well documented</a> and there is an <a href="http://www.voiceglue.org/pipermail/voiceglue/" target="_blank">active community of users</a> offering tips and troubleshooting advice.

Hats off to the people behind VoiceGlue &#8212; Doug Campbell and Steve Smith. Well done!