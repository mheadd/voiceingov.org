---
id: 20
title: Detecting Answering Machines
date: 2005-06-19T16:45:24+00:00
author: mheadd
layout: page
guid: http://www.voiceingov.org/blog/?page_id=20
original_post_id:
  - "20"
---
**Wait for the beep&#8230;**

Applications that utilize outbound calling, whether done through services offered by [VoiceXML hosting providers](http://cafe.bevocal.com/docs/svc_outnotification/index.html) or [through CCXML,](http://docs.voxeo.com/ccxml/1.0/appendixf_ccxml.htm) must have some way of dealing with answering machines. This can be a tricky business, because the number of different types of answering machines (to say nothing of voice mail, answering services, etc.) makes it hard to create a standard interface for identifying machines and responding accordingly.

There is an [interesting tutorial](http://studio.tellme.com/notifier/ansmach/ansmach.html) available on answering machine detection from the TellMe Studio site. The following paragraphs and sample code are my own humble attempt to address this pesky dilemma. Please feel free to use (and modify) the code described on this page. Future thanks for sharing any enhancements you make with me.

**Assumptions**

If we assume that customers are aware that from time to time they may get a call with information about a service (e.g., a campground or park reservation, notice to make a payment), then it should be fairly easy to place an out bound call that presents the user with an introductory message. This message will include a VoiceXML form that accepts only a specific sequence of dtmf keys as a valid response &#8211; something only a human could understand and interact with. If this assumption is reasonable, then the approach described below for detecting answering machines should be fairly straightforward to follow.

**The Answering Machine Detection Script**

The file presents the caller with an introductory message, and accepts only *-9-8 as a valid entry. An answering machine message should throw a nomatch event &#8212; the nomatch logic checks the length of the call and tests it against a pre-established limit. If the call limit has not been exceeded, a reprompt is thrown but there is no text in the second prompt. This means that there will be no strange utterances left on the answering machine of the call recipient if the answering machine starts recording before the threshold is exceeded.

Any utterance (or lack of utterance) that doesn&#8217;t match the grammar continues this cycle until the threshold is exceeded. It then transitions to a listener form that waits to hear the end of the answering machine message. After 2.5 seconds of silence (following, hopefully, the answering machine beep) it transitions to a message form that plays the message for the call recipient&#8217;s answering machine.

[Download the script](../xfiles/tutorials/ans_machine_detect.vxml)

(Note &#8211; you may notice that this file includes the BeVocal DTD. I make use of a BeVocal extension &#8211; the session variable [&#8220;session.bevocal.timeincall&#8221;](http://cafe.bevocal.com/docs/vxml/variables.html#274638)&#8211; to make this work. As such, the BeVocal DTD is required and it must be run on the BeVocal platform.)

<!-- Creative Commons License -->

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/"><img alt="Creative Commons License" border="0" src="http://creativecommons.org/images/public/somerights20.gif" /></a>
  
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/">Creative Commons Attribution-NonCommercial-ShareAlike 2.5 License</a>.

<!-- /Creative Commons License -->