---
id: 438
title: (Not So) New Voice Application Development Tools
date: 2009-01-05T20:26:48+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=438
permalink: /not-so-new-voice-application-development-tools/
jd_tweet_this:
  - 'yes'
wp_jd_clig:
  - http://cli.gs/ypttgT
original_post_id:
  - "438"
categories:
  - Development Tools
---
Today I came across a couple of new services that aim to make the development of voice applications easier. The question I have after reading about each is: why do we need these?

The first, <a href="http://voicephp.com/" target="_blank">VoicePHP</a>, is from <a href="http://tringme.com/" target="_blank">TringMe</a> &#8211; a company that provides voice services and an API that developers can use to integrate telephony functions into web applications (e.g., click-to-call). The other, <a href="http://www.twilio.com/" target="_blank">Twilio</a>, is a service that let&#8217;s application developers use an XML-based markup language to develop telephone applications.

To be fair, I have not fully evaluated either service, but I will be developing test applications over the coming days and commenting further on each as deemed necessary (still waiting on my trial account to fully dig into VoicePHP).

### VoicePHP

In a nutshell, VoicePHP <a href="http://voicephp.com/whyvoicephp.html" target="_blank">extends the PHP scripting language</a> for voice applications:

> VoicePHP will automagically give voice to all existing PHP APIs and language constructs. For example:
> 
> `echo "Hello World";`
> 
> will speak &#8220;Hello World&#8221; instead of printing it

In addition, VoicePHP appears to add several new PHP language constructs that are specific to voice applications. I believe that these are intended to enable functionality for telephone-specific functions not present in the core PHP language (e.g., conducting a transfer). The deployment model for applications built with VoicePHP is not entirely clear &#8212; traditional web apps built with PHP render markup that is consumed by a web browser. Where are VoicePHP applications executed? In a plain old Apache web server like other PHP applications? If so, how are <acronym title="Text-To-Speech">TTS</acronym> and <acronym title="Automatic Speech Recognition">ASR</acronym> handled? How is integration with the telephony environment handled?

The idea of abstracting away the details of VoiceXML markup and using a mainstream programming language like PHP to build voice applications <a href="http://www.phpmagazine.net/docs/voicexml/" target="_blank">is not a new one</a>. I&#8217;ve [personally made use](http://www.voiceingov.org/blog/?p=150) of the fantastic <a href="http://www.hawhaw.de/" target="_blank">HAWHAW library</a> from Norbert Huffschmid to build voice and mobile applications, and anyone seriously considering building a voice application with PHP should give it a look. So the question becomes, what does VoicePHP offer that existing approaches to using PHP to create voice applications do not?

I will admit that the basic premise of VoicePHP is attractive &#8212; I&#8217;m eager to try it. However, until my trial account is activated I&#8217;ll just have to wait and offer what thoughts I have.

### Twilio

<a href="http://www.twilio.com/" target="_blank">Twilio</a> is a service that allows developers to build voice applications using an XML-based language. Twilio applications are executed in a hosted environment where implementation details like telephone number provisioning and interacting with the PSTN are handled through a simple web interface. The Twilio environment interacts with an application platform via HTTP.

Developers can use a variety of different languages and platforms to interact with the hosted platform (i.e., generating the TwiML markup that performs telephony functions and user dialogs) &#8212; PHP, Ruby, Python, etc. The choice of a whether to use a back end database, integrate with other systems, interact with web services, etc. is left to the developer and all of these details occur outside the Twillo execution environment. In a nutshell, Twillo aims to provide a simple way to develop and deploy a voice interface to web applications.

If that description sounds eerily close to the Raison d&#8217;être for VoiceXML then you&#8217;re in the same place that I am.

It&#8217;s not immediately clear to me what hole in the voice application development tool set Twilio is intended to fill. This is exactly the purpose for the development of XML-based dialog languages like VoiceXML. Companies like Voxeo, BeVocal, TellMe, VoiceGenie and others have provided hosting services similar to what Twillo provides [for a while now](http://www.voiceingov.org/blog/?p=146) &#8212; and they&#8217;re pretty good at it too.

If VoiceXML is viewed as being too complex or to unweildy for a voice application, I would strongly recommend looking at a language like <a href="http://docs.voxeo.com/callxml/3.0/" target="_blank">CallXML</a>, which has a pretty healthy user base and a growing company like <a href="http://www.voxeo.com/" target="_blank">Voxeo</a> behind it. CallXML is also close enough to <a href="http://www.gnutelephony.org/index.php/BayonneXML" target="_blank">other dialog languages</a> that it might be possible to port them to other platforms. Twilio is the one and only company that supports TwiML.

Don&#8217;t get me wrong, Twilio sounds pretty cool and I&#8217;m working on my first Twilio app right now. I&#8217;m just not sure how (or if) it&#8217;s going to change my life as a voice application developer.