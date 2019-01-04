---
id: 87
title: 'Combating the &#8220;Alpha Bail&#8221;'
date: 2006-03-27T13:53:44+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=87
permalink: /combating-the-alpha-bail/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "87"
categories:
  - General Discussion
---
In a [very interesting piece](http://www.speechtechmag.com/issues/ 11_1/human_factor/12763-1.html) in the January/February issue of Speech Technology Magazine, Walter Rolandi discusses the problems of speech recognition with individual letters of the English alphabet.

> One [problem] is that all but one letter (W) is monosyllabic. Another reason is that so many of the letters in the English alphabet sound essentially the same. 

The most common instance where a speech application might need to collect individual English letters from a caller is probably name and address capture. Because this can be excruciatingly difficult to do, a number of techniques and tools have been developed to assist voice application developers.

The first approach &#8211; which I term the &#8220;lookup approach&#8221; usually involves the capture of a caller&#8217;s ANI and a name/address lookup using a [third party service](http://community.voxeo.com/services_pro/targus/usage.jsp). This is an easy, efficient way of capturing name and address information but it isn&#8217;t full proof. Callers that are not listed in publicly available directories may not be candidates for a lookup, and the services themselves are not free.

An alternative &#8211; which I have very unscientifically termed the &#8220;prebuilt approach&#8221; usually involves the construction of a set of pre-built grammars and other components that can be put in place to support a name and address capture. These components &#8211; like the Open Speech Dialog Modules, or OSDM&#8217;s, available from Nuance &#8211; are typically incorporated into voice applications as subdialogs. They are usually made up of a large collection of individual street-level grammar files that may be precompiled to run faster on a production platform. They&#8217;re usually pretty complex, but they act sort of like a black box in your voice application &#8211; the caller is handed off to the subdialog and the collected values are <return>ed for post call processing. As with the lookup approach, this technique isn&#8217;t full proof. Since street names and address can change over time (think suburban sprawl) this approach won&#8217;t work for every caller. Additionally, since street listings get stale with time, a long-term subscription to a vendor might be required.

The universal alternative to these approaches is human transcription of recorded audio files. When a lookup or a prebuilt component doesn&#8217;t work, a voice application will typically fall back to audio recording for transcription later by a human employee. The obvious downside with this approach is the time and cost of transcription.

There are other alternatives, but for many of the reasons outlined by Rolandi most commercial grade applications will employ one of the approaches outlined above.

Many a voice developer has posited the question; &#8220;Why not just build a DTMF grammar that let&#8217;s a caller spell out words using their keypad?&#8221; Good question and one that I have taken on in my spare time to satisfy my curiosity about how efficient and accurate such an approach might be. The result of my humble laboring can be <a href="http://www.voiceingov.org/xfiles/spellTest.txt" target="_blank">found here</a>.

In taking this exercise on, I&#8217;ve decided to take a bit of a different approach &#8212; building a DTMF grammar that let&#8217;s a caller spell out words isn&#8217;t all that challenging. Building a voice interface that uses such a grammar gracefully and efficiently most definitely is. 

What I have tried to do is to build a simple, yet graceful VoiceXML application that allows callers to spell words. My goal was to make it as simple as possible, but also to make it highly flexible. In undertaking my little exercise, I&#8217;ve decided to keep my grammars simple &#8211; in fact, I decided to let callers enter DTMF input for spelling words with nothing but a simple builtin &#8220;digits&#8221; grammar.

I accomplish this by using a JavaScript array that has all of the allowable values for spelling words with DTMF entry in it. I&#8217;m not sure it&#8217;s the best way of approaching this problem, but it&#8217;s definitely unique.

I&#8217;ve also tried to structure my application by using both voice and DTMF entry for spelling words. There will be times when an ASR engine will do pretty good in recognizing letters. This won&#8217;t always be the case, so if ASR fails the application falls back to DTMF entry.

I&#8217;m definitely not finished &#8211; I suspect that this will be a work in progress for a while. I&#8217;d like to add an additional component that works with the confirmation dialog to speak a word out that begins with the letter the caller entered:

> Caller says: &#8220;K&#8221;
  
> Application says: &#8220;Did you say K, as in Karate?&#8221; 

I&#8217;d also like to add logic that would cause the application to fall back permanently to DTMF entry if the ASR engine gets the wrong letter after a certain number of attempts.

If you&#8217;ve got any thoughts, feedback or suggestions please feel free to [share them](mailto:mheadd@voiceingov.org) with me.