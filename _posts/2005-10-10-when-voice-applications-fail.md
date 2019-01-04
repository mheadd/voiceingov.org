---
id: 66
title: When Voice Applications Fail
date: 2005-10-10T10:27:08+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=66
permalink: /when-voice-applications-fail/
categories:
  - General Discussion
---
Itâ€™s always frustrating to hear people complain about poorly designed telephone applications, particularly when these complaints relate to government-sponsored applications. (Encouraging good design principles for developing voice applications is kind of the point of this here blog â€“ â€˜ya dig?)

You can imagine how disheartened I was to read a [story cataloging a litany of complaints](http://www.delawareonline.com/apps/pbcs.dll/article?AID=/20051010/NEWS/510100346) about one such system in my hometown newspaper. [This article](http://www.delawareonline.com/apps/pbcs.dll/article?AID=/20051010/NEWS/510100346) focuses on some extremely negative experiences by callers to the Medicare help line (1-800-MEDICARE). In an analysis of the Medicare help line, the General Accounting Office [concluded late last year](http://www.consumeraffairs.com/news04/medicare_gao.html) that less than two thirds of callers to this â€œhelpâ€ line received accurate answers to their questions.

To be perfectly fair, there are few things in this world â€“ with the possible exception of the U.S. Tax Code â€“ as complex as the [Medicare](http://www.medicare.gov/default.asp) program. Not a surprise considering itâ€™s the largest health insurance program in the nation, covering some 40 million Americans (and counting!). When eligibility and benefit rules are complex, the job of providing clear concise answers to recipient questions is difficult, so some degree of frustration is probably inevitable.

Itâ€™s also more likely that a phone system set up to answer benefit questions will have to utilize a mixture of automated dialogs and customer service representatives. Given the nature of the program, developing a fully automated system would seem to be highly impractical.

However, when I read about some of the negative caller experiences, I canâ€™t do anything but shake my head:

> After two minutes of following instructions from an automated female voice, Bingham arrived at a list of choices.
> 
> &#8220;If you&#8217;re calling on the Medicare discount drug cards say, &#8216;Drug card,&#8217; &#8221; the voice said. &#8220;For Medicare health plans, say &#8216;Plan choices.&#8217; &#8221;
> 
> &#8220;Plan choices,&#8221; Bingham said.
> 
> &#8220;I&#8217;m sorry I didn&#8217;t understand you,&#8221; the voice said.
> 
> &#8220;Plan choices!&#8221; Bingham said again.
> 
> &#8220;I&#8217;m sorry, I still didn&#8217;t understand you.&#8221;
> 
> Bingham brought the telephone two inches from her lips.
> 
> &#8220;PLAN CHOICES!&#8221;
> 
> &#8220;Please hold a moment while I transfer you to a customer service representative who can help you.&#8221;

This is just a fundamental lack of proper menu construction and grammar tuning â€“ the fact that these things do not appear to have been done for an application as heavily used as this one is almost criminal. At a minimum, the menu should accept both spoken and DTMF input so that a caller can use their key pad to enter a numeric choice if the application is having trouble recognizing what they are saying. 

`<br />
POORLY DESIGNED MENU STRUCTURE:</p>
<p><menu><br />
<noinput>I'm sorry I didn't understand you.</noinput><br />
<nomatch>I'm sorry I didn't understand you.</nomatch><br />
<prompt>Welcome to the Medicare help line.  If you're calling on the Medicare discount drug cards say, Drug card.  For Medicare health plans, say Plan Choices.<br />
</prompt><br />
   <choice next="#drugs">drug cards</choice><br />
   <choice next="#choice">plan choices</choice><br />
</menu><br />
` 

This type of menu structure has several flaws. First, the prompts are too dissimilar to the menu choices â€“ if the desired input for the second item is â€œplan choices,â€ why doesnâ€™t the prompt direct the user to this input? â€œFor Medicare health plans, say Plan Choices.â€ should be â€œFor Medicare health plans, say Health Plans.â€ This increases the likelihood that the caller will provide the right input.

Second, the menu does not allow for approximate input â€“ if a caller says simply â€œcards,â€ or â€œdiscount cardsâ€ the application will not recognize the input. Under the VoiceXML 2.0 specification, the [default setting](http://www.w3.org/TR/voicexml20/#dml2.2.1) for menu input is â€œexactâ€ &#8212; in other words, the VoiceXML interpreter will look for an exact match on the menu items, and will not recognize input that includes some (but not all) of the words in the menu item.

Third, the menu does not allow for DTMF entry, which would allow a caller to fall back to their key pad for entry if the application was having trouble recognizing their input. Properly constructed voice applications will check for the type of input being provided, and direct callers to modify it accordingly (demonstrated below).

Finally, the menu does not use tapered prompts to assist the caller in refining their input, or selecting the appropriate input type.

`<br />
PROPERLY DESIGNED MENU STRUCTURE:</p>
<p><!-- Accept attribute is set to allow approximate input --><br />
<menu accept="approximate"></p>
<p><!-- Tapered prompts when no input is detected --><br />
<noinput count="1"><br />
<prompt>I'm sorry. I didn't hear what you said. </prompt><br />
<reprompt/><br />
</noinput></p>
<p><noinput count="2"><br />
<prompt>I'm having trouble hearing you.  If you are using a speaker phone, you may want to pick up your telephone handset or make sure your phone is not on mute</prompt><br />
<reprompt/><br />
</noinput></p>
<p><noinput count="3"><br />
<prompt>I'm sorry I'm still having trouble hearing you.  Please wait while I transfer you to a customer service representative.</prompt><br />
<goto next="#transfer" /><br />
</noinput></p>
<p><!-- Tapered prompts when application does not recognize input --></p>
<p><nomatch count="1"><br />
<prompt>I'm sorry I didn't understand what you said. </prompt><br />
<reprompt/><br />
</nomatch></p>
<p><nomatch count="2"><br />
  <if cond=" application.lastresult$.inputmode='voice'"><br />
     <prompt>I'm still having trouble hearing you.  Please try entering your selection using your touch tone key pad. </prompt><br />
<reprompt /><br />
&nbsp;&nbsp;<else/><br />
<prompt>I didn't get that.  Please try entering your selection again. </prompt><br />
<reprompt /><br />
  </if><br />
</nomatch></p>
<p><nomatch count="3"><br />
<prompt>I'm sorry.  I'm still having trouble understanding your selection.  Please wait while I transfer you to a customer service representative.</prompt><br />
<goto next="#transfer" /><br />
</nomatch></p>
<p><prompt>Welcome to the Medicare help line.<br />
<enumerate><br />
For the Medicare <value expr="_prompt"/> service, say <value expr="_prompt"/>, or press <value expr="_dtmf"/>.<br />
</enumerate><br />
</prompt></p>
<p><!-- menu choices allow for alternate DTMF input --><br />
   <choice next="#drugs" dtmf="1">discount drug cards</choice><br />
   <choice next="#choice" dtmf ="2">health plans</choice><br />
</menu>`

Additionally, a good system will verify user input if the confidence level on recognition is lower than a pre-specified threshold. There is a good overview of this technique in an [earlier posting](http://www.voiceingov.org/blog/?page_id=33) on this site.

This is pretty basic stuff â€“ even if the application developers didnâ€™t know enough to take this approach when they built the system, it is most certainly something that should have been uncovered during testing. This is the kind of second rate development that gives voice applications a bad rep.