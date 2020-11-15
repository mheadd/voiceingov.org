---
id: 31
title: AJAX in VoiceXML Applications
date: 2005-07-01T10:09:33+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=31
permalink: /ajax-in-voicexml-applications/
categories:
  - General Discussion
---
With all of the buzz of late around [AJAX](http://en.wikipedia.org/wiki/AJAX) (an acronym first coined by [Jesse James Garrett](http://www.adaptivepath.com/publications/essays/archives/000385.php) to stand for Asynchronous JavaScript and XML), it was only a matter of time before the discussion began about using this approach in VoiceXML applications. There is an [interesting article](http://www.voicexmlreview.org/columns/Jun2005_speak_listen.html) in the new VoiceXML review covering this topic.

The approach described in this article makes use of the new <data> element that is part of the VoiceXML 2.1 specification. I'm not clear on whether this approach is the same as AJAX, but it is indeed powerful.

AJAX applications rely on a JavaScript object called "[XMLHttpRequest](http://www.xml.com/pub/a/2005/02/09/xml-http-request.html)" that allows a web interface to communicate with a backend server without transitioning to new page. More traditional web applications submit user input to, or request data from, a backend server using the [HTTP GET and POST methods](http://www.w3.org/2001/tag/doc/whenToUseGet.html). The transmission of this data and the subsequent transition to a new page can sometimes be perceived as disjointed and inelegant by users. AJAX allows web interfaces to fetch or submit data without transitioning to a new page, making them feel smoother (I guess).

The VoiceXML 2.1 [<data> element](http://www.w3.org/TR/2005/CR-voicexml21-20050613/#sec-data) doesn't work exactly like this. It allows a developer to designate an XML document to be fetched by a VoiceXML interpreter as part of a dialog. It then exposes that document via the [JavaScript binding to the Document Object Model](http://www.w3.org/TR/DOM-Level-2-Core/ecma-script-binding.html). Its "AJAX-like" in the sense that it obviates the need for a page transition to render the data in the XML document, but there isn't any asynchronous communication with a backend server through an XMLHttpRequest object.

Given the recent hype, I can understand the desire to characterize the functionality provided by the new 2.1 spec as emulating AJAX. But I think it's important to point out that callers to a VoiceXML application probably don't perceive page transitions in the same way that users of a visual web application do. There are [ways to manage user reactions](http://cafe.bevocal.com/docs/vxml/fetching.html) to these transitions within VoiceXML and users need to be aware of how to use them effectively.