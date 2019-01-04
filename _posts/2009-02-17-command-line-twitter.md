---
id: 691
title: Command Line Twitter
date: 2009-02-17T09:53:39+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=691
permalink: /command-line-twitter/
original_post_id:
  - "691"
categories:
  - Linux
  - Tutorials
  - Twitter
---
Just when I thought it couldn&#8217;t get any easier to send out a Tweet, I lucked out and found <a href="http://www.commandlinefu.com" target="_blank">Command-line Fu</a>.

While browsing some very cool command line tricks, I happened upon a command to send out a Tweet using `curl` from the command line. I&#8217;ve seen this kind of example before &#8211; there may even be something similar in the Twitter API Wiki &#8211; but for some reason it resonated with me this morning. After playing around with it a bit, I&#8217;ve tweaked it to my liking:

`curl -s -u user:password -d status="$1" http://twitter.com/statuses/update.xml > /dev/null`

You can drop this into a file using your favorite editor. Save it and make sure the file is executable (`chmod u+x fileName`). You can execute this file at the command line like this:

`$ ./fileName "This is the text of my Tweet."`

I&#8217;ve opted to redirect the output returned by executing the `curl` command (Twitter will respond to the request with an XML document) to the <a href="http://en.wikipedia.org/wiki/Data_sink" target="_blank">bit bucket</a>. I&#8217;ve also opted to turn off the normal progress indicator used by `curl` by invoking the `-s` flag. Feel free to tweak this to your heart&#8217;s desire.

I&#8217;ll definitely be sending out more Tweets from the command line, and I&#8217;ll definitely be going back over to <a href="http://www.commandlinefu.com" target="_blank">Command-line Fu</a> for some more command line tricks.