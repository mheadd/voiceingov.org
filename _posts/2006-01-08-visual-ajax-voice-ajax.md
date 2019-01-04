---
id: 79
title: Visual AJAX != Voice AJAX
date: 2006-01-08T22:40:20+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=79
permalink: /visual-ajax-voice-ajax/
categories:
  - Standards
---
There is an [interesting article](http://www.speechtechmag.com/issues/11_1/technology_trends/12752-1.html) in the latest installment of Speech Technology Magazine â€“ Jim Larson offers his thoughts on the use of the [AJAX development technique](http://en.wikipedia.org/wiki/Ajax_(programming)) in VoiceXML applications.

Larson points to [a notable example](http://www.voicexmlreview.org/columns/Jun2005_speak_listen.html) of &#8220;AJAX-like&#8221; programming in VoiceXML. In fact, its one [I have commented](http://www.voiceingov.org/blog/?p=31) on before. My view of this technique hasn&#8217;t changed. It&#8217;s powerful, but it isn&#8217;t AJAX.

The way I view AJAX programming, there are two key hallmarks; processing of XML on the client using JavaScript and the <acronym title="Document Object Model">DOM</acronym>, and asynchronous communication with a back end server using HTTP. The approach referenced by Larson includes the former, but misses the latter.

Make no mistake, using the new [VoiceXML 2.1 <data> element](http://www.w3.org/TR/2005/CR-voicexml21-20050613/#sec-data) to fetch an XML document that can be utilized from within a VoiceXML script without a page transition is indeed handy. But there isn&#8217;t any asynchronous communication happening here â€“ the external XML document is fetched only once, [typically at the begining](http://www.w3.org/TR/2004/REC-voicexml20-20040316/#dml6.1) of script execution. For asynchronous behavior, we need to look to one of VoiceXML&#8217;s sister technologies â€“ the [Call Control eXtensible Markup Language](http://www.w3.org/TR/2005/WD-ccxml-20050111/) (CCXML).

The CCXML 1.0 specification (a last call working draft from the W3C) includes the [<send> element](http://www.w3.org/TR/2005/WD-ccxml-20050111/#Send), which allows a developer to throw a user defined event that can be caught by the initiating (or another) CCXML document. The classic <send> example involves the throwing of a â€œtimesUpâ€ event that allows a developer to designate when a call should be terminated.

The good folks at Voxeo (leaders in the effort to formalize the CCXML standard) have added functionality to their platform that allows the <send> element to asynchronously [send and receive information via HTTP](http://docs.voxeo.com/ccxml/1.0/frame.jsp?page=appendixg_ccxml.htm). So its theoretically possible to construct a conference call application that sends a message to a back end script to kick off an IM message to the call organizer when there are 30 seconds left on the call. (Actually, that sounds pretty cool â€“ may have to try that one soon.)

When it comes to AJAX in VoiceXML, the same functionality is generally available but the needs of developers are somewhat different. Callers to a VoiceXML application probably donâ€™t perceive page transitions in the same way that users of a visual web application do. VoiceXML has [fetching and caching functionality built into it](http://www.voicexmlreview.org/Dec2002/features/vxml_app_performance.html) that can be used to manage the delay between page transitions. It&#8217;s probably a better use of developer time to understand fetching and caching more intimately than to try and replicate AJAX in voice applications.