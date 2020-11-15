---
id: 93
title: Detecting Caller Frustration
date: 2006-06-10T11:21:31+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=93
permalink: /detecting-caller-frustration/
categories:
  - Standards
  - Tutorials
---
There are not a lot of really good tools for IVR developers to detect when callers are getting frustrated. Anger is a human emotion, and human emotions are complex. The tools generally used within VoiceXML applications for dealing with frustrated callers tend to be a bit ham-fisted. 

For example, most developers utilize a graduated set of noinput/nomatch handlers for transferring callers who are having problems to an agent. Additionally, it is also possible to detect when a certain type of input on the part of the caller is causing problems (e.g., voice input) and to direct them utilize another (e.g., DTMF) &#8212; there is a more complete discussion of this approach [here](http://www.voiceingov.org/blog/?p=90), and [here](http://www.voiceingov.org/blog/?p=66). 

We can not detect frustration by looking at the volume, pitch or prosody of spoken input within VoiceXML "“ at least not yet.

However, the new VoiceXML 2.1 specification provides another tool that we can use to try and detect when callers are becoming testy. The <a href="http://www.w3.org/TR/voicexml21/#sec-mark" target="_blank"><mark> element</a> is a new VoiceXML element that allows developers to determine when caller bargin occurs. This can be very handy for detecting when callers are becoming frustrated with repetitive prompts (like confirmation dialogs that ask the caller to confirm what they have said or entered).

By using the <mark> element judiciously, we can make reasonable assumptions about when callers are getting sick of confirming input, and act accordingly. A sample script with some <mark>&#8217;s in it <a href="http://www.voiceingov.org/xfiles/markTest.txt" target="_blank">can be found here</a>. 

This sample contains a customer satisfaction survey that asks the caller to rate (from 1 to 5) their agreement with several statements. After each turn, the caller is asked to confirm their answer &#8211; you can see how this could become a bit of a pain for a caller, especially if they are not happy to begin with.

The following bit of code does the neat stuff:

`<br />
<if cond="confirmation$.markname=='conf_start' && confirmation$.marktime < 1500"><br />
<assign name="confirm" expr="false"/><br />
</if><br />
` 

At the beginning of our confirmation dialog, we insert a <mark> with a name of &#8216;conf\_start&#8217;, and at the end we insert a <mark> with a name of &#8216;conf\_end&#8217;. Platforms that support this element will expose some useful information &#8212; the name of the last <mark> executed, and the time since its execution before the caller barged in. 

The conditional statement above checks to see what the name of the last executed <mark> is. If the name is &#8216;conf\_start&#8217; we know that the &#8216;conf\_end&#8217; mark was not reached (the caller barged in before the prompt was done). We also check the time since the <mark> was executed before bargein occurred &#8212; if it is a relatively short period (the caller barged in quickly) we can assume that they are growing frustrated. We therefore turn off our confirmation flag (setting the variable &#8220;confirm&#8221; to false) so that the caller is not asked to conform any more input.

There are lots of ways to get creative with the <mark> element &#8212; we could use it to end the survey all together, we could use it to trigger a shorter, more concise set of prompts, etc. Generally speaking, there is no silver bullet for detecting caller frustration, but with the growing number of tools available (including the <mark> tag), the job is getting easier.