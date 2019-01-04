---
id: 730
title: Ubuntu Asterisk Oddness
date: 2009-02-27T19:13:58+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=730
permalink: /ubuntu-asterisk-oddness/
original_post_id:
  - "730"
categories:
  - Asterisk
  - Linux
  - Open Source
---
I have a virtual machine running Ubuntu 8.10 Server and I&#8217;ve been meaning to give <a href="http://www.voiceglue.org/" target="_blank">VoiceGlue</a> a try to see if I could set up my own completely open source VoiceXML platform,

I found that I was able to run `sudo apt-get install asterisk` at the command line, and I started to get excited. This was going to be the easiest Asterisk install yet. I was very soon disabused of this foolish notion.

The Asterisk install seemed to go smoothly, as did the basic set up and config. Just to make sure I was doing things by the numbers I set up a couple of extensions and a quick test to have Festival read something back to me. So far, so good. Next it was on to the VoiceGlue install.

Following the instructions in the <a href="http://voiceglue.org/wiki/doku.php" target="_blank">VoiceGlue Wiki</a>, the install went smoothly. All three VoiceGlue-related services started just fine (the `voiceglue` service itself barked at me because I had not yet set up call routing in /etc/voiceglue.conf &#8211; once I did this, it started up just fine.)

That&#8217;s when things got weird. The VoiceGlue Wiki says:

> Phoneglue also needs to be contacted via FastAGI for all calls that it will handle, and it needs to use a particular context, extension, and priority to send calls to itself. 

OK, no worries there. I set up a new context in /etc/asterisk/extensions.conf and then reloaded the dialplan from the Asterisk console. So far so good. Then, the oddness set in.

I kept seeing an error in the Asterisk logs saying:

> `res_agi.c:229 launch_netscript: Connect to 'agi://localhost' failed: Connection refused`

After banging my head against the wall trying to figure it out I decided to check and see if anything was listening on port 4573 (the default port for FastAGI). No dice.

I tried running the test AGI script comes with Asterisk (agi-test.agi). Again, no dice. In fact, it doesn&#8217;t look like there are any directories containing AGI scripts anywhere.

Ubuntu Asterisk seems to be looking in /usr/share/asterisk/agi-bin/ &#8211; it doesn&#8217;t exist. Neither does the usual directory for AGI scripts (/var/lib/asterisk/agi-bin/). Nor does another common directory &#8211; /var/spool/asterisk/outgoing.

Why is all of this missing from the Ubuntu version of Asterisk? Anyone have any thoughts? Did I miss something obvious during the install?

I&#8217;m still eager to try VoiceGlue, so it looks like I&#8217;ll be building Asterisk from scratch.