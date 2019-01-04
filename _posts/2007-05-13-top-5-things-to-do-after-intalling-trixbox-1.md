---
id: 115
title: 'Top 5 Things to do after intalling Trixbox: #1'
date: 2007-05-13T09:39:27+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=115
permalink: /top-5-things-to-do-after-intalling-trixbox-1/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "115"
categories:
  - trixbox
  - VoIP
tags:
  - Asterisk
---
Today I&#8217;ll start a series that I&#8217;ve been meaning to commit to writing for some time. The top 5 things to do after you finish installing Trixbox.

[Trixbox 2.2](http://www.trixbox.org/downloads) is now out, so if you&#8217;ve ever thought about setting up your own VoIP system I would strongly recommend giving Trixbox a try. The Trixbox [forums](http://www.trixbox.org/forum) provide a wealth of information, and there is some [documentation](http://forge.trixbox.org/gf/project/trixbox2/wiki/?action=index) available from the Trixbox project itself. However, most of the documentation focuses on the installation process, and does not provide a whole lot of information about what to do after your system is installed and running.

With this in mind, I&#8217;d like to share some of the things that I think are important to ensuring that your Trixbox system is running smoothly (and securely).

First things first &#8212; we need to lock down **<acronym title="Secure Shell">SSH</acronym>**.

If your not familiar with SSH, you should [brush up](http://www.suso.org/docs/shell/ssh.sdf) on this remote connection method. Right out of the gate SSH is a more secure way of connecting to a Linux machine than alternatives like Telnet, but that doesn&#8217;t mean it&#8217;s perfect.

At a minimum, the following steps should be taken to improve SSH security:

  * Run SSH on an alternate port (the default is port 22).
  * Only use the SSH 2 protocol (SSH 1 is not as secure as 2)
  * Do not allow root logins via SSH (in fact, it&#8217;s a much better approach to only allow specifically named users to log in via SSH, but never root).
  * Use public key authentication, instead of passwords.

There are a number of other helpful tips and tricks available [here](http://enterprise.linux.com/enterprise/07/03/26/1423232.shtml?tid=129&tid=100&tid=35).

Next up, we&#8217;ll walk through the process of backing up a Trixbox system and transferring the backup to a remote machine using scp and cron.