---
id: 326
title: Screen Pops with Asterisk and XMPP
date: 2008-12-23T19:10:45+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=326
permalink: /screen-pops-with-asterisk-and-xmpp/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - 'An example of how to generate screen pops with Asterisk, PHP and XMPP.  Open Source + Open Standards = Awesome! '
wp_jd_clig:
  - http://cli.gs/917ynQ
original_post_id:
  - "326"
categories:
  - Asterisk
  - Standards
tags:
  - Google
  - OpenFire
  - PHP
  - Screen Pop
  - XMPP
---
A few weeks ago, I wrote a post on [using CCXML and PHP to do screen pops](http://www.voiceingov.org/blog/?p=156) with the <a href="http://www.voxeo.com/prophecy/" target="_blank">Voxeo Prophecy</a> platform. The task was made incredibly easy with a nifty PHP class library <a href="http://code.google.com/p/xmpphp/" target="_blank">designed specifically to interact with XMPP servers</a>.

After getting screen pops working in Prophecy, I decided to try my hand at getting them to work in Asterisk (this is, after all, how the majority of phone calls to me are handled). It turns out, the PHP script I developed to do screen pops in Prophecy can be reused to do the very same thing in Asterisk. If you&#8217;d like to give this a try on your own, here is what you will need:

  1. A working Asterisk instance. (This example assumes you know how to modify Asterisk config files.)
  2. A copy of the very cool <a href="http://code.google.com/p/xmpphp/" target="_blank">XMPPHP library</a> from Google Code. (This example assumes that the PHP code used to interact with XMPP servers is running on a separate server than the one housing Asterisk. More on this below.)
  3. An <a href="http://www.igniterealtime.org/projects/openfire/index.jsp" target="_blank">XMPP server</a> and a client, or simply use your Google account.

There are three components to this example. First, you&#8217;ll need a PHP script to interact with an XMPP server. You can use the same script employed in my previous screen pop example:

`<br />
connect();<br />
$conn->processUntil('session_start');<br />
$conn->message('someguy@gmail.com', 'You are receiving a call from: $ani');<br />
$conn->disconnect();<br />
?><br />
` 

Next, you&#8217;ll need a simple AGI script for Asterisk to interact with. This script will send an HTTP request to the PHP script above:

`<br />
#!/bin/bash<br />
# Contents of screen_pop.agi<br />
# It don't get any easier than this...<br />
curl http://ip_address_to_your_web_server/screen_pop.php?ani="$1"<br />
` 

On your Asterisk server, place this script in `/var/lib/asterisk/agi-bin/` and ensure that it is executable. You&#8217;ll need to specify the IP address to the server running this script. Note &#8212; there isn&#8217;t any reason that these two scripts can&#8217;t run on the same machine (you can run PHP scripts on an Asterisk server), or even be consolidated into one single script (just make sure to include the XMPPHP library). In fact, you _could_ even run the XMPP server used to send screen pops on the same machine running Asterisk.

The mechanics of this specific example were influenced by the set up for my previous screen pop example, and I am (at heart) a lazy basterd. But I digress&#8230;

The last piece to be put in place is to modify `extensions.conf` to ensure that our AGI script is invoked. You&#8217;ll want to add something like the following to the appropriate dial plan context (your specific Asterisk set up will influence this heavily):

`exten => 123,1,AGI(screen_pop.agi|${CALLERID(num)})` 

This will pass the caller ID to our AGI script, which will then invoke the PHP script and send the details of the call to the XMPP account of our choice. I&#8217;ve noticed in practice that adding this to my dial plan causes the IM to be sent to my Google Talk account a good 2-3 seconds before the phone rings. Plenty of time to give someone a heads up about who is on the other end of an incoming call.

Obviously there are lots of options for looking up information on the caller, once you have their caller ID, that you can use to augment the information in your screen pop.

Just goes to show you, there isnâ€™t much you canâ€™t do with open source / open standards.

Viva screen pop!