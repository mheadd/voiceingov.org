---
id: 153
title: Building a Custom Stats Monitor for Voxeo Prophecy
date: 2008-11-05T18:12:22+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=153
permalink: /building-a-custom-stats-monitor-for-voxeo-prophecy/
original_post_id:
  - "153"
categories:
  - Development Tools
  - Digital Divide
  - Tutorials
tags:
  - AJAX
  - Prophecy
  - utilities
  - Voxeo
---
I do a lot of work on the <a href="http://www.voxeo.com/prophecy/" target="_blank">Voxeo Prophecy platform</a>. Pretty regularly, I have a need to check and see how many ports are in use, or how many active sessions are currently running on a Prophecy machine.

Prophecy has some nice features that make checking these stats pretty easy. You can get a view of currently running sessions by pointing your browser at _sessions_10_ on either the CCXML or VoiceXML control port &#8212; for example, if you want to check CCXML sessions you would point to http://prophecy\_ip:9999/sessions\_10. There is also another utility that provides a wealth of information on what is happening on your Prophecy server &#8212; _stats_10_.

This utility is particularly useful because you can pass in query string parameters and customize the information that is returned, as well as the format that it is rendered in. Pointing your browser to http://prophecy\_ip:9999/stats\_10?type=counters&format=xml gets you a list of currently utilized ports and running CCXML sessions in a compact XML document. As handy as this is, because its browser-based, you have to keep hitting refresh to get updated stats from your server. Since this can be inconvenient, I decided to whip up a small AJAX-powered stats monitor for Voxeo Prophecy.

**Requirements / Considerations:**

  * You&#8217;ll need to download the <a href="http://xajaxproject.org/" target="_blank">xajax PHP class library</a> to use this utility. I downloaded 0.5 RC 2 Full &#8212; you can do this by entering the following at the Linux command line: _wget http://xajaxproject.org/download/0.5\_rc2/xajax\_0.5\_rc2\_full.zip_. Extract these files to a folder where PHP scripts can be executed. 
<img style="float:right;padding-left:10px;" src="http://www.voiceingov.org/blog/monitor_off.jpg" />

  * Before running this monitor, you&#8217;ll want to make sure that you can run xajax applications &#8212; do this by testing out the helloworld.php sample in the xajax/examples directory. I developed this app on Ubuntu 8.10. It should run just fine on Windows &#8212; if you want to run this on the same machine as Prophecy you should use a separate PHP installation, not the one that comes bundled with Prophecy.

**Get the Source Code:**

You can [get the code](http://www.voiceingov.org/tutorials/prophecy_stats_monitor.zip) for the custom stats monitor [here](http://www.voiceingov.org/tutorials/prophecy_stats_monitor.zip).

When downloaded, extract the files to the same directory where you placed your xajax files. Open up the file called **voxeo.php** in an editor and modify the configuration options as needed. The value of _$interval_ determines how frequently an AJAX callback will be made to your Prophecy server to check stats. You will want to change the value of _$prophecyHost_ to the IP address of your Prophecy server. You can add stats items to the _$counterNames_ array if you want to check more than the basic level of stats. Save and close this file when finished.

<img style="float:right;padding-left:10px;" src="http://www.voiceingov.org/blog/monitor_on.jpg" />

Open up the **voxeo.php** file in your web browser. When the page loads, the stats monitor is not yet started &#8212; click &#8220;Start Monitor&#8221; to engage. When you make a test phone call to your Prophecy server, you will see the numbers change for ports and sessions.

Prophecy 9 is <a href="http://evolution.voxeo.com/forums/main.jsp?bb-cid=85&bb-tid=718099" target="_blank">on the horizon</a>, and the good folks at Voxeo will likely be adding all sorts of eye candy that will make monitoring your Prophecy server a snap. Still, it is nice to know that the Prophecy platform is open enough to support custom utilities that can help manage your server.