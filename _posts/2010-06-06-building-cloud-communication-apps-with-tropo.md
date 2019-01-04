---
id: 1817
title: 'Building Cloud Communication Apps with Tropo: Part 1'
date: 2010-06-06T12:58:42+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1817
permalink: /building-cloud-communication-apps-with-tropo/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "1817"
categories:
  - Development Tools
  - Tropo
  - Tutorials
  - VoIP
tags:
  - IM
  - PHP
  - SIP
  - SMS
  - Twitter
---
A few months back, I wrote a series of posts on [building NoSQL telephony applications with Tropo and CouchDB](http://www.voiceingov.org/blog/?p=1525). Today I&#8217;m going to start a continuation of that series, focusing on how to build cutting edge cloud communications apps with the Tropo WebAPI.

**What is the Tropo WebAPI?**

The Tropo WebAPI is, in a nutshell, an <a href="https://www.tropo.com/docs/webapi/overview.htm" target="_blank">HTTP/JSON API</a> for building multi-channel communication applications &#8211; applications that you interact with via phone, IM, SMS or Twitter. While my earlier series on Tropo focused on building applications in Tropo&#8217;s [scripting environment](http://www.voiceingov.org/blog/?p=1625) (another fine option for developers), this series will focus on building JSON-based applications (generated using PHP) that can be hosted anywhere and executed in the Tropo cloud environment.

Faithful readers will recognize some similarities here to a post I did a while back on the [HTTP/JSON API provided by CloudVox](http://www.voiceingov.org/blog/?p=1625), another cloud telephony provider. While the concept behind these two API&#8217;s is very similar, there are some key differences that make Tropo a highly attractive option for developers.

First, the Tropo service is truly multi-channel &#8211; using the Tropo WebAPI you can build applications that work on a range of different communication channels, not just phones (although you can build some pretty slamming phone apps as well).

Since I&#8217;m a phone app developer at heart, some of the features that Tropo provides for phone applications really get me excited. Tropo supports both DTMF entry and speech recognition. It also has broad multilingual support. In addition, Tropo gives phone application developers the ability to <a href="http://blogs.voxeo.com/speakingofstandards/2008/05/06/what-is-a-p-header-in-sip-and-whyhow-would-you-use-one/" target="_blank">manipulate SIP headers</a>, an important feature in building sophisticated cloud communication apps that I hope to demonstrate down the road a bit.

**Getting Started**

Head on over to <a href="https://www.tropo.com/" target="_blank">Tropo.com</a> and set up a new account (if you don’t have one already). Take a little time to review the documentation for the Tropo WebAPI. For the example applications in this series of blog posts I&#8217;ll be using <a href="http://github.com/tropo/tropo-webapi-php" target="_blank">a PHP class library I developed</a> specifically to interact with the Tropo WebAPI.

The crew behind Tropo have provided a <a href="http://github.com/voxeo/tropo-webapi-ruby" target="_blank">Ruby Gem</a> for interacting with the Tropo WebAPI. However, since I like to do my cloud telephony work with PHP I decided to write my own set of classes for doing this. Whether you&#8217;re a Ruby-head or a PHP enthusiast, using one of these tools to generate JSON for consumption by the Tropo WebAPI can make build an application significantly easier, particularly as you get into more sophisticated application development.

You can get the PHP Library, as well as some of the sample apps we&#8217;ll be looking at, from GitHub:

> $ git clone git://github.com/tropo/tropo-webapi-php.git 

You&#8217;ll need to host these classes and the PHP scripts you write with them on a server that can be accessed from the Tropo environment. Any web server that supports PHP will do.

**My First Tropo WebAPI Application**

Let&#8217;s start with the standard Hello World app:

> `<br />
Say("Hello World!");</p>
<p>// Render the JSON for the Tropo WebAPI to consume.<br />
$tropo->RenderJson();</p>
<p>?><br />
` 

You can look at the rendered JSON in your browser, and you should see something like this:

> `<br />
{<br />
&nbsp;&nbsp;&nbsp;&nbsp;"tropo":&nbsp;[<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"say":&nbsp;[<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"value":&nbsp;"Hello&nbsp;World!"<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;]<br />
}<br />
` 

Go to the <a href="https://www.tropo.com/applications/" target="_blank">Applications</a> section in your Tropo account and set up a new WebAPI application that points to the location of this script.

<img src="http://localhost:8000/wp-content/uploads/2010/06/application_step_1.png" alt="Create a new Tropo WebAPI application" title="Create a new Tropo WebAPI application" width="385" height="224" style="margin:5px;padding:7px;" />

<img src="http://localhost:8000/wp-content/uploads/2010/06/application_step_2.png" alt="Assign a URL to your new Tropo WebAPI application" title="Assign a URL to your new Tropo WebAPI application" width="385" height="200" style="margin:5px;padding:7px;" />

When you create your application, Tropo will automatically provision a Skype number, a SIP number and an <a href="http://www.inum.net/" target="_blank">iNum</a>. You can additionally add a PSTN number in a range of different area codes at no charge.

You may also notice the section below the provisioned phone numbers entitled &#8220;Instant Messaging Networks&#8221; &#8211; this section allows you to set up any number of different IM accounts (and Twitter!) that your application can use. We&#8217;ll dive deeper into this in future posts.

For now, we&#8217;ll keep it simple and use the auto provisioned Skype number &#8211; when you call this number, you will hear it say &#8220;Hello World.&#8221;

The next post in this series will focus on a more sophisticated application that uses the <a href="http://github.com/tropo/tropo-webapi-php" target="_blank">TropoPHP classes</a> and the utterly awesome <a href="http://www.limonade-php.net/" target="_blank">Limonade PHP framework</a>.

Stay tuned&#8230;