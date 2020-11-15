---
id: 118
title: Die Zombies Die!
date: 2007-05-16T12:25:07+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=118
permalink: /die-zombies-die/
categories:
  - Development Tools
  - General Discussion
---
Yes. I admit it. I created a zombie. Very recently in fact. It was ugly (embarrassingly so) and, like all zombies, very hard to kill once it had been created.

No, this is not an excerpt from a <a href="http://en.wikipedia.org/wiki/Image:Zombies_on_broadway.jpg" target="_blank">Bela Lugosi movie</a> "“ I refer to something far more horrifying. A zombie session in CCXML.

The route to avoiding zombie sessions is pretty straightforward "“ just follow the rules, set up your handlers properly and all will be well. Occasionally, however, mistakes are made and applications are not tested as thoroughly as they should be.

So as some self imposed penance, I&#8217;ve decided to list some tried and true methods for avoiding zombie sessions. Nothing new here, but it helps to go over this stuff every once in a while.

**Always <exit> properly**

When catching errors, be very careful not to get fancy, especially with a handler that is meant to catch multiple types of errors.

> <transition event=&#8221;error.*&#8221;>
  
> <exit/>
  
> </transition> 

Some people try and get cute here, and execute logic inside this transition "“ be very careful. If the logic has an error in it, you will enter a loop and have a really bad day. I don&#8217;t even like to put log statements in this kind of transition. Log files are generally verbose enough to tell you what the problem is.

**Order <transition>s carefully**

When a CCXML file is executed, <transition> elements are executed in document order "“ even though a more appropriate <transition> for an event exists in a document, the CCXML platform will execute the first <transition> that matches the event/condition in document order. So, even though you place your catch all error handler (like the one above) at the end of your file, if you have another problem in a different <transition>, it may never get executed. For example, consider the following:

> <transition event=&#8221;error.semantic&#8221;>
  
> <log expr="&#8217;This log statement has an error&#8217;">
  
> <!&#8211; A semantic error in another part of my CCXML app will get me stuck here &#8211;>
  
> <exit/>
  
> </transition>
> 
> <transition event=&#8221;error.*&#8221;>
  
> <exit/>
  
> </transition> 

**Set up your zombie killer in advance**

Make it standard practice to send an event to the currently running session when the page loads to close things down after a certain period of time if the session doesn&#8217;t end normally.

> <transition state=&#8221;&#8216;initial'&#8221; event=&#8221;ccxml.loaded&#8221;>
  
> <send event=&#8221;&#8216;sessionEnd'&#8221; target=&#8221;session.id&#8221; delay=&#8221;&#8216;600s'&#8221;/>
  
> </transition>
> 
> <transition event=&#8221;sessionEnd&#8221;>
  
> </exit>
  
> </transition> 

**Test. Test again. Test some more&#8230;**

The best way to ensure that your not going to create some zombie sessions (or do anything else really stupid) is to test thoroughly.

The best way to do this "“ in my humble opinion "“ is to download a copy of <a href="http://www.voxeo.com/prophecy/" target="_blank">Voxeo Prophecy</a> and use it to develop your application. You can use Prophecy to test your application in your own environment before moving it to Voxeo hosting, or upgrading to a production version of Prophecy to run your app.

Be careful. Follow these steps. Avoid all zombies. Good luck!