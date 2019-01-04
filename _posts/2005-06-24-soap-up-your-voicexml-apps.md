---
id: 27
title: SOAP up your VoiceXML Apps
date: 2005-06-24T07:58:00+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=27
permalink: /soap-up-your-voicexml-apps/
categories:
  - General Discussion
---
Both the [BeVocal](http://cafe.bevocal.com) and [Voxeo](http://community.voxeo.com) VoiceXML platforms incorporate the Simple Object Access Protocol (SOAP) and Web Services Definition Language (WSDL) specifications to provide some interesting options for VoiceXML developers.

Both platforms allow developers to create a proxy object for a web service by referencing a WSDL file for a given service. By doing this, developers can then access the methods of a web service as if it were simply a JavaScript object. The proxy object handles all the conversion necessary to transform a call to a JavaScript function to a SOAP method.

This functionality provides another way for developers to separate the presentation layer (in the case of a voice application, the rendering of VoiceXML) from the business layer. It also allows VoiceXML developers to access the [wealth of different services available](http://www.xmethods.net/) from web services providers. Developers should pay close attention to the implementation details provided on both he [Voxeo](http://www.vxml.org/frame.jsp?page=appendixo.htm) and [BeVocal](http://cafe.bevocal.com/docs/vxml/soap.html#270560) sites.

One caveat, it appears that at least the BeVocal implementation works only with RPC-oriented services, not document-oriented services (support for document-oriented services is planned for a later release). Not sure if the Voxeo platform supports document-oriented services.