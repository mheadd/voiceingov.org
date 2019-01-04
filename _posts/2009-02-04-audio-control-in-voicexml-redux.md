---
id: 666
title: 'Audio Control in VoiceXML: Redux'
date: 2009-02-04T18:52:45+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=666
permalink: /audio-control-in-voicexml-redux/
original_post_id:
  - "666"
categories:
  - Development Tools
  - Open Source
  - Standards
---
> One of the questions I hear most from VoiceXML developers relates to audio control features in VoiceXML. VoiceXML does not natively support the ability to &#8220;rewind&#8221; or &#8220;fast forward&#8221; through audio files, but some vendors provide this functionality as extensions on their platforms. 

Does this mean you can&#8217;t implement audio control in a VoiceXML application? No, it doesn&#8217;t.

Over 2 years ago, I wrote a post [describing how to achieve audio control in VoiceXML](http://www.voiceingov.org/blog/?p=109) in a way that was portable and platform agnostic. I continue to think that this is the right approach for achieving audio control in voice applications.

VoiceXML is a markup language that can be used in conjunction with any number of server side languages: PHP, Perl, Java, C#, Ruby, etc. If it can be used to build a traditional web app, it can be used to build a VoiceXML app as well.

I think it&#8217;s great that <a href="http://code.voicephp.com/getting-past-the-voicexml-limitations-voicephp-audio-player/" target="_blank">other platforms</a> are providing innovative ways to provide language-specific methods for achieving audio control in voice applications.

But the true power of VoiceXML is that it is standards based, runs on a large number of platforms, and won&#8217;t shoehorn you into a programming language that may not be right for you.

VoiceXML = Flexibility, Portability, Opportunity!