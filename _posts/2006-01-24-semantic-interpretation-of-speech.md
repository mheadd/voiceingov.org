---
id: 82
title: Semantic Interpretation of Speech
date: 2006-01-24T08:37:11+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=82
permalink: /semantic-interpretation-of-speech/
categories:
  - Standards
---
A couple of weeks ago, the W3C released the Semantic Interpretation for Speech Recognition (SISR) specification as a [candidate recommendation](http://www.w3.org/TR/semantic-interpretation/). This is an important step forward in the development of the Speech Interface Framework, and will help further define the differences between voice applications built using VoiceXML and its sister technologies, and [other ways](http://www.oreillynet.com/pub/a/etel/2005/12/19/hacking-in-asterisk-and-rails.html) of building voice applications.

To fully understand Semantic Interpretation, a little background is helpful. VoiceXML applications utilize grammars to define allowable utterances for caller interactions. The job of a speech recognition platform is to match as closely as possible the spoken input of a caller against a list of allowable utterance defined in a grammar, or to determine that there is no good match (in which case a &#8220;nomatch&#8221; event is thrown). When a successful match is made, a grammar can return to a VoiceXML application the exact sequence of words that was spoken (i.e., to fill a <field> variable) or it can return a representation of that information in another format.

Semantic Interpretation is the process of converting the raw text of spoken utterances into a representation of data that is more easily processed by computer programming languages.

> In an application, knowing the sequence of words that were uttered is sometimes interesting but often not the most practical way of handling the information that is present in the user utterance. What is needed is a computer processable representation of the information, the Semantic Result, more than a natural language transcript. 

To some extent, the facility to do this already exists within VoiceXML and the Speech Recognition Grammar Specification (SRGS). Most seasoned developers know that a specified value can be returned to a voice application from a grammar based on a match [using the <tag> element](http://www.w3.org/TR/2004/REC-speech-grammar-20040316/#S2.6). The value returned by the <tag> can be further processed by the application, as opposed to using the string of text that was matched from the grammar.

An example of a grammar that uses this approach <a href="http://www.voiceingov.org/grammars/grammar_test3_xml.txt" target="_blank">can be viewed here</a>. Note that when a caller speaks a name that is matched in this grammar, the value defined within the <tag> element is returned to the voice application for further processing (in this case, the value is returned to a <field> designated with a slot value of &#8220;department&#8221;).

The purpose of the SISR specification is to define an expanded syntax of the Semantic Interpretation result. Understanding how these results will structured is critical for making use of them within the larger context of a voice application. 

Developers interested in Semantic Interpretation should also check out information on this topic in the [VoiceXML 2.0](http://www.w3.org/TR/2004/REC-voicexml20-20040316/#dml3.1.5) specification and the [SRGS 1.0](http://www.w3.org/TR/2004/REC-speech-grammar-20040316/#S1.4) specification.