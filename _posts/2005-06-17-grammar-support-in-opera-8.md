---
id: 12
title: Grammar Support in Opera 8
date: 2005-06-17T06:38:26+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=11
permalink: /grammar-support-in-opera-8/
categories:
  - Standards
---
Conflicting posts in newsgroups dealing with the new multimodal browser released from Opera raise the question of which grammar formats are supported in the new browser.

[This post](http://www.voicexml.org/cgi-bin/ubb/ultimatebb.cgi?ubb=get_topic;f=7;t=000006) on the VoiceXML Forum Community Message Board bemoans the lack of support for the SRGS (Speech Recognition Grammar Specification) format for speech grammars in Opera 8. It suggests that the new browser only supports the JSGF (Java Speech Grammar Format).

This would be a major issue in my opinion, as [the SRGS specification](http://www.w3.org/TR/speech-grammar/) is closely tied to the VoiceXML standard â€“ both were developed under the auspices of the W3C, both were adopted as standards on the same day and the certification of both [developers](http://www.voicexml.org/certification/developer.html) and [platforms](http://www.voicexml.org/certification/platform.html) by the VoiceXML Forum is contingent on support for the SRGS. If one of the benefits of XHTML+Voice is that it will leverage the existing skills of VoiceXML developers, why wouldn&#8217;t the first XHTML+Voice browser support the designated standard for grammars under the VoiceXML specification?

After further research, I came across [this post](http://my.opera.com/forums/showthread.php?s=c77dd1d8426abe986c9dc010c6aebed8&threadid=83252) on the Opera Developer forum. It states pretty clearly that the SRGS spec is indeed supported in Opera 8. The only way to be sure, however, is to set up a quick test.

Using [one of the examples](http://my.opera.com/community/dev/voice/xv-script/) on the Opera developer site, which is designed to use a JSGF grammar, I modified the example to use an SRGS grammar (in XML format). These two sample files can be tested by pointing the  [latest version of the Opera Browser](http://my.opera.com/mheadd/affiliate/) at the links below:

  * [Grammar Test 1](http://www.voiceingov.org/files/x_plus_v/grammar_test1.xml) (using JSGF grammar)
  * [Grammar Test 2](http://www.voiceingov.org/files/x_plus_v/grammar_test2.xml) (using SRGS grammar)

Both work identically, demonstrating that the new [XHTML+Voice enabled browser from Opera](http://my.opera.com/mheadd/affiliate/) does indeed support the SRGS grammar format.