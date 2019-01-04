---
id: 110
title: Building Better IVRs
date: 2007-02-15T14:06:33+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=110
permalink: /building-better-ivrs/
categories:
  - General Discussion
---
Anyone who builds IVRs for a living is aware of the <a href="http://www.gethuman.com/" target="_blank">gethuman project</a>. This project, started by a fellow named Paul English in response to a fairly crappy phone service experience by a member of his family, aims to provide consumers with information on how to bypass IVR systems.

The project has also developed a <a href="http://www.gethuman.com/standard/v1.0.html" target="_blank">classification and rating system</a> to grade the IVRs of the top 500 organizations it maintains information about. Not surprisingly, most of the organizations ranked received an F on their current phone systems. Those organizations that seemed to do well (based on my very unscientific review of the gethuman 500 database) appear to bypass IVRs entirely and go directly to an agent.

There are some <a href="http://www.gethuman.com/us/index.html#government" target="_blank">government entities</a> on this list, all of which are federal agencies of one sort or another. Since almost every government entity (both large and small) is now utilizing some sort of IVR system, and since the telephone is still one of the <a href="http://www.pewinternet.org/PPF/r/128/report_display.asp" target="_blank">primary methods</a> for citizens to contact their government, it would behoove governments to take notice of the gethuman standard.

Since VoiceXML is the defacto standard for developing IVR systems, I believe that some of the criteria included in the gethuman standard can be translated directly into best practices for coding VoiceXML applications. The following is a quick summary of suggested coding practices to meet specific gethuman ranking criteria. (Note, not all of the criteria used to rank IVR systems under the gethuman standard can be addressed using VoiceXML coding techniques. As such, the following represents a subset of the relevant gethuman criteria.)

**1. The caller must always be able to dial 0 or to say &#8220;operator&#8221; to queue for a human.**

> The VoiceXML <link> element is the best way to provide a global transfer option. Using <link>, we can easily create a way for a caller to transfer to a representative at any point during an IVR.
> 
> The following example shows a link element that will fire off a user defined event called â€œhelp&#8221; when either the 0 DTMF key is pressed, or the caller says any of the items defined in the grammar. Placing this call in the application root document gives it scope in any part of the IVR application:
> 
> <link dtmf=&#8221;0&#8243; event=&#8221;help&#8221;>
  
> <grammar mode=&#8221;voice&#8221; version=&#8221;1.0&#8243; root=&#8221;R_1&#8243;>
      
> <rule id=&#8221;R_1&#8243; scope=&#8221;public&#8221;>
         
> <one-of>
           
> <item>transfer</item>
           
> <item>operator</item>
           
> <item>representative</item>
           
> <item>help</item>
         
> </one-of>
      
> </rule>
    
> </grammar>
  
> </link>
> 
> We also need a handler for this event in the same scope as the <link>:
> 
> <catch event=&#8221;help&#8221;>
    
> <assign name=&#8221;callerSelection&#8221; expr=&#8221;Transfer&#8221;/>
    
> <prompt>Please hold for the next available representative.</prompt>
    
> <goto next=&#8221;#transfer&#8221;/>
  
> </catch>
> 
> If you give this <link> application-level scope, you need to be conscious of fields where you may be using similar grammar elements. So, for example, if your IVR has a field in it that also uses the 0 DTMF key or one of the words in the <link> grammar, make sure that you specify which grammar the field should use by setting the â€œmodal&#8221; attribute on your <field> item.

**2. Callers should never be asked to repeat any information (name, full account number, description of issue, etc.) provided to a human or an automated system during a call.**

> This is more a <acronym title="Computer Telephony Integration">CTI</acronym> issue than anything else, but there are some things that can be done within a VoiceXML application to make sure that the appropriate information is passed to an agent when a call is transferred.
> 
> The VoiceXML specification describes the â€œaai&#8221; attribute of the <transfer> tag. This attribute allows a VoiceXML application to pass data to a called party. Additionally, some VoiceXML platforms allow a developer to set the ANI when making a transfer:
> 
> <transfer dest=&#8221;tel:+13159999999;ani=8077894563&#8243;/>
> 
> This way, an agent does not need to ask the caller (again) for their telephone number. Unfortunately, both of these features are somewhat unevenly implemented across platforms. Improved support for passing information along with a call transfer is certainly one area where VoiceXML could be improved.

**3. Speech applications should provide touch-tone (DTMF) fall-back.**

> In a VoiceXML application, this boils down to setting the &#8220;inputmodes&#8221; property. [I&#8217;ve written on this before](http://www.voiceingov.org/blog/?p=90) in a previous post where I lay out options for detecting the type of input mode, and changing the &#8220;œinputmodes&#8221; property within an application where appropriate.

**4. Callers should not be forced to listen to long/verbose prompts.**

**5. Callers should be able to interrupt prompts (via dial-through for DTMF applications and/or via barge-in for speech applications) whenever doing so will enable the user to complete their task more efficiently.**

> These two criteria are related, and deserve to be considered in unison. VoiceXML developers can allow a caller to break into a prompt either by setting the global &#8220;bargein&#8221; property, or by setting the &#8220;bargein&#8221; attribute on a <prompt> tag.
> 
> Bear in mind that an Automatic Speech Recognition (ASR) engine will usually attempt a recognition when what it thinks is spoken input is detected; even if this is just background noise, static or a caller mumbling. Without some careful thought and planning, allowing indiscriminate bargein can cause more problems than it solves, particularly for speech applications.
> 
> To allow only DTMF input to bargein, set the &#8220;bargein&#8221; property appropriately:
> 
> <property name=&#8221;bargein&#8221; value=&#8221;dtmf&#8221;/>
> 
> Additionally, using the â€œbargeintype&#8221; property can help ensure that bargains only occur when there is an active grammar matched. Finally, you can use the â€œmarktime&#8221; property of the application.lastresult$ object to determine where in a prompt a caller barged in (this is a new feature oF VoiceXML 2.1). This can be helpful in determining If the bargein was accidental or purposeful.

**6. Do not disconnect for user errors, including when there are no perceived key presses (as the caller might be on a rotary phone); instead queue for a human operator and/or offer the choice for call-back.**

> Although there arenâ€™t a lot of rotary callers out there, there are some. And since handling multiple noinputs/nomatches is pretty straightforward in VoiceXML, there is no reason not to include this in your applications:
> 
> <catch event=&#8221;noinput nomatch&#8221; count=&#8221;3&#8243;>
    
> <prompt> Please hold while I transfer you.</prompt>
    
> <goto next=&#8221;#transfer&#8221;/>
  
> </catch>

Despite some protestations to the contrary, IVRs do have their place in the customer service equation. Used correctly governments can go a long toward improving the service they provide to citizens by implementing IVR systems.

However, the governments that use IVRs and the developers that build them must be aware of their limitations, and design appropriately for callers who cannot (or will not) talk to a computer.