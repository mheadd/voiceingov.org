---
id: 83
title: E4X in VoiceXML
date: 2006-02-06T08:04:59+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=83
permalink: /e4x-in-voicexml/
categories:
  - General Discussion
  - Standards
---
Voice application developers familiar with the proposed VoiceXML 2.1 specification are aware of new functionality that allows external XML documents to be accessed from within VoiceXML scripts. The new [<data> element](http://www.w3.org/TR/2005/CR-voicexml21-20050613/#sec-data) will allow developers to fetch XML data from a web server without transitioning to a new VoiceXML document (a handy trick indeed). The XML data that is fetched is exposed as a read-only subset of the [W3C Document Object Model (DOM)](http://www.w3.org/TR/DOM-Level-2-Core/ecma-script-binding.html) and can be manipulated using ECMAScript (JavaScript).

While I&#8217;m very excited about this new functionality, using the DOM is not really my favorite thing in the world to do â€“ I&#8217;ve always found it a bit clunky and hard to get really comfortable with. Using the DOM is not the only way to manipulate XML from within JavaScript. There is another ECMA standard that allows developers to do this â€“ [ECMAScript for XML (or E4X)](http://developer.mozilla.org/en/docs/E4X).

E4X is an extension to JavaScript that adds native XML support to the language. With E4X, in addition to [native object types](http://developer.mozilla.org/en/docs/Core_JavaScript_1.5_Reference#Global_Objects) in JavaScript like the Number type, the String type, the Boolean type, there is the XML type for representing XML elements, attributes, comments, processing-instructions or text nodes and there is the XMLList type for representing list of XML objects. 

E4X is an [official ECMA standard](http://www.ecma-international.org/publications/standards/Ecma-357.htm), but right now support for it is kind of sparse. There are a couple of JavaScript engines (not surprisingly, both from Mozilla) that support it, but it isn&#8217;t supported yet in any of the standard browser releases. If you want to use E4X, you have to download one of the nightly builds from the Mozilla download site. Hopefully this will change soon, and E4X support will become standard in JavaScript engines.

As the W3C and the VoiceXML community move toward final adoption of the VoiceXML 2.1 standard, it may be worth considering weather the DOM is the only (or best) way that voice developers should be able to access and manipulate XML data from within VoiceXML scripts. Choice is a good thing &#8212; hint, hint. 😉