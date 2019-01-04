---
id: 138
title: JavaScript Trick for Voice Applications
date: 2008-04-25T17:40:45+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=138
permalink: /javascript-tricks-in-voice-applications/
original_post_id:
  - "138"
categories:
  - Tutorials
---
There are times when it is desirable to change the behavior of a VoiceXML application based on a specific setting.

For example, the GreenPhone application that I have mentioned in [several](http://www.voiceingov.org/blog/?p=135) [previous posts](http://www.voiceingov.org/blog/?p=136) has a setting that can be used to control whether special audio files are played. I personally find these audio files funny and somewhat endearing &#8212; others may not. To control whether they are played, there is a variable in the application root document called (cleverly) **playAudio**.
  
`<br />
<var name="playAudio" expr="true"/><br />
` 
  
It&#8217;s default setting is **true**, and this can be changed to **false** to prevent these files from playing. The typical method for checking a variable like this one to determine if an audio file should be played looks something like this:
  
`<br />
<if cond="playAudio"><br />
&nbsp;&nbsp;<audio src="myFile.wav"/><br />
</if><br />
` 
  
There isn&#8217;t anything wrong with this, and since there isn&#8217;t a &#8220;cond&#8221; attribute on the <audio/> tag there aren&#8217;t very many good alternatives. There is one alternative method that I rather fancy that uses the [JavaScript conditional operator](http://developer.mozilla.org/en/docs/Core_JavaScript_1.5_Reference:Operators:Special_Operators:Conditional_Operator) to distill this to a single line of code:
  
`<br />
<audio expr="playAudio ? 'myRealAudioFile.wav' : 'myFakeAudioFile.wav'"/><br />
` 
  
This shortcut allows us to assign a value to the audio file reference via the &#8220;expr&#8221; attribute, instead of using an explicit URI to the location of an audio file. The way the operator behaves is to first evaluate the condition on the far left side &#8212; if it evaluates to **true** then the first expression is assigned as the URI of the audio file. If it evaluates to **false**, then the second expression is used.

The trick here is that the second expression resolves to a bogus audio file &#8212; it doesn&#8217;t exist. This will not cause a fatal error in your application, it will simply cause Prophecy not to play an audio file (it can&#8217;t because the file doesn&#8217;t exist).

The JavaScript conditional operator can come in very handy in CCXML as well. For example, there are times in CCXML where I want to use <dialogterminate/> to end a call, but I may not be certain which dialog a caller is in &#8212; the JavaScript conditional operator can come in handy here:
  
`<br />
<dialogterminate dialogid="loggedIn ? voiceMailDialog : loginDialog"/><br />
` 
  
Since the &#8220;dialogid&#8221; attribute is an expression, we can use the JavaScript conditional operator to check and see if a caller has logged into a voice mail system to retrieve their voicemail. If there loggedIn status is true, we assume that they are in the voiceMailDialog and yank them from that. Otherwise, we assume they are in the first dialog and yank from there.

There are surely other ways to do these things, but in my humble opinion the JavaScript conditional operator deserves some attention as a powerful shortcut for doing things in CCXML or VoiceXML using the [Voxeo Prophecy platform](http://voxeo.com/prophecy/).