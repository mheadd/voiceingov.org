---
id: 782
title: Shoring up Asterisk Security
date: 2009-04-07T14:51:10+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=782
permalink: /shoring-up-asterisk-security/
jd_tweet_this:
  - 'yes'
wp_jd_clig:
  - http://cli.gs/JEa3Z6
original_post_id:
  - "782"
categories:
  - Asterisk
  - Linux
  - Open Source
---
Found out today that an external host had been scanning my Asterisk server looking for valid SIP extensions. Turned out the IP belonged to some German hacking site that was probably using some brute force tools to scan my server (and lots of others) for valid SIP extensions. The ultimate goal was more than likely to try and exploit any live extensions for some free phone calls.

Fortunately, in anticipation of moving my in-house Asterisk server <a href="http://voxilla.com/2009/02/13/asterisk-amazon-ec2-1178" target="_blank">out to the cloud</a> I had recently done some work to become better educated on Asterisk security and to shore up the security of the CentOS machine my Asterisk instance is running on. As a result, my intrusion detection system slammed the door to the external scans pretty quick, and I&#8217;ve since added the IP address to my iptables rule set to to drop any requests from the IP used for the scan.

It was a little unnerving to find out that my box was getting scanned, but I&#8217;m glad I took the time recently to get things working more securely. This incident reminds me that one can never be too careful about security, and that there is **always** more to learn about running Asterisk more securely. To underscore this last point, here are some great links I&#8217;ve come across lately for Asterisk and Linux security:

  * <a href="http://www.junctionnetworks.com/blog/mike/2009/03/25/weak-passwords-on-extensions-equals-hacked-box" target="_blank">Weak Passwords on Extensions Equals Hacked Box</a>
  * <a href="http://www.voipusersconference.org/forum/topics/john-todds-security-list" target="_blank">John Todd&#8217;s Security List</a>
  * <a href="http://nerdvittles.com/?p=580" target="_blank">Avoiding the $100,000 Phone Bill: A Primer on Asterisk Security</a>

Some general Linux security reading:

  * <a href="http://www.iptablesrocks.org/" target="_blank">An iptables Guide and Tutorial</a>
  * <a href="http://www.linux.com/feature/61061" target="_blank">Advanced SSH security tips and tricks</a>

Happy reading!