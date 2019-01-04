---
id: 156
title: CCXML, XMPP and PHP
date: 2008-12-05T18:20:45+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=156
permalink: /ccxml-xmpp-and-php/
original_post_id:
  - "156"
categories:
  - Standards
  - Tutorials
---
Ever want to build an IVR application that could do <a href="http://en.wikipedia.org/wiki/Screen_pop" target="_blank">screen pops</a>? Want to build it using open standards / open source software? Guess what; you can. Here&#8217;s how&#8230;

  1. Download yourself a copy of <a href="http://www.voxeo.com/prophecy/" target="_blank">Voxeo Prophecy</a>
  2. Grab a copy of the very cool <a href="http://code.google.com/p/xmpphp/" target="_blank">XMPPHP library</a> from Google Code
  3. Get yourself an <a href="http://www.igniterealtime.org/projects/openfire/index.jsp" target="_blank">XMPP server</a> and a <a href="http://www.igniterealtime.org/projects/spark/index.jsp" target="_blank">client</a>, or simply use your Google account.

I won&#8217;t go over all of the gory details here on how to build a screen pop capable IVR, but if you can build a simple CCXML app to accept an inbound call, you&#8217;re more than ready to do this on your own. You can also <a href="http://docs.voxeo.com/ccxml/1.0-final/home.htm" target="_blank">check out the Voxeo tutorials</a> for pointers.

On an inbound call, I usually grab the caller&#8217;s <acronym title="Automatic Number Identification">ANI</acronym> in order to make it part of the screen pop:
  
`<br />
<transition state="'init'" event="connection.alerting"><br />
&nbsp;<assign name="ani" expr="event$.connection.remote"/><br />
&nbsp;<send target="'screen_pop.php'" targettype="'basichttp'" namelist="ani"/><br />
</transition><br />
` 

Sending this information to a simple PHP script built with the XMPPHP library allows you to generate a screen pop via an instant message to the XMPP account of your choice:
  
`<br />
connect();<br />
$conn->processUntil("session_start");<br />
$conn->message("someguy@someserver.net", "You are receiving a call from: $ani");<br />
$conn->disconnect();<br />
?><br />
` 

How cool is that!

Obviously there are lots of options for looking up information on the caller, once you have their ANI, that you can use to augment the information in your screen pop.

Just goes to show you, there isn&#8217;t much you can&#8217;t do with open source / open standards.

Viva screen pop!