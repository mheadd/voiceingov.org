---
id: 186
title: 'My Favorite Linux Commands: find'
date: 2008-12-14T16:45:14+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=186
permalink: /favorite-commands-find/
jd_tweet_this:
  - 'yes'
wp_jd_clig:
  - http://cli.gs/9ByMzs
original_post_id:
  - "186"
categories:
  - Linux
---
Over the next week or so (as time allows), I&#8217;ll attempt to write a short summary of my favorite Linux commands. These are commands that I use regularly and make my life a lot easier. Hopefully, they&#8217;ll make your life easier too. Up first, the `find` command.

Not surprisingly, you can use the `find` command to locate files (and other things ;-)). What is surprising is the large number of options that you can set to find exactly the file(s) that you are looking for, and take action on them.

Let&#8217;s say that I have a directory called &#8220;sale&#8221; with JPEG files in it that have differences in their file extension (i.e, some were saved as \*.jpg, and some as \*.JPG) and their permission settings. I&#8217;d like to know how many of each I have, and then I&#8217;d like to change the permissions on a subset of them, to make those that are currently not readable by the &#8220;other&#8221; group readable.

First, lets see how many files are in the directory:

`$ find sale/ -type f -iname "*.jpg"<br />
sale/P9130285.JPG<br />
sale/P9130286.JPG<br />
sale/P9130287.JPG<br />
sale/P9130288.JPG<br />
sale/P9130289.JPG<br />
sale/P9130290.JPG<br />
sale/P9130291.JPG<br />
sale/phone_1.jpg<br />
sale/phone_2.jpg<br />
sale/phone_3.jpg<br />
sale/phone_4.jpg<br />
sale/phone_5.jpg`

Like other Linux commands, you can direct the output of `find` into another command to get a summary count of all the files located:

`$ find sale/ -type f -iname "*.jpg" | wc -l<br />
12`

In the above example, I use the `-type` option and the `-iname` option to list all files with a .jpg extension (regardless of case). Not surprisingly, using the `-name` option applies a similar, but case sensitive, pattern match on file names:

`$ find sale/* -type f -name "*.jpg"<br />
sale/phone_1.jpg<br />
sale/phone_2.jpg<br />
sale/phone_3.jpg<br />
sale/phone_4.jpg<br />
sale/phone_5.jpg`

There are a large number of options that can be used, and the real power of this command comes from a thorough understanding of them. One of the most useful, in my opinion, is the `-exec` option. This option lets you do something specific with each file that is found. To change the permissions on all files with a case sensitive &#8220;.jpg&#8221; extension and make the files readable by the &#8220;other&#8221; group, you could use the following:

`find sale/* -type f -name "*.jpg" -exec chmod o+r {} ;`

The `-exec` option lets you specify another command that is run on each file found (represented by the {} argument). Note, you also need to end the command issued by `-exec` with an escaped semicolon. Using the `find` command in this way can be enormously helpful for doing things like clearing out old log files. I typically use `find` in a cron job that searches directories for log files older than a certain amount of time (using the `-mtime` option) and then deletes them from the directory:

`find logs/ -type f -name "*log*" -mtime +30 -exec rm {} ;`

If you&#8217;re like me you will probably have an arsenal of commands that you use on a regular basis, to quickly and easily do the things that you do almost every day. Having the `find` command in your arsenal can make life a lot easier.