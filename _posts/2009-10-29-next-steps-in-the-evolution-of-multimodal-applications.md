---
id: 1314
title: Next Steps in the Evolution of Multimodal Applications
date: 2009-10-29T13:57:52+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1314
permalink: /next-steps-in-the-evolution-of-multimodal-applications/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "1314"
categories:
  - Development Tools
  - Twitter
  - VoIP
---
Over at <a href="http://europe.ecomm.ec/" target="_blank">eComm Europe</a> – being held in Amsterdam – RJ Auburn gave a rocking presentation that can be summed up by its very apt title – the &#8220;<a href="http://www.slideshare.net/voxeo/the-rise-of-realtime-text-and-the-demise-of-voice" target="_blank">Rise of Real Time Text and the Demise of Voice</a>.&#8221;

There are many important take aways in this presentation for governments and any other organization that interfaces with customers (yes, taxpayers and citizens are customers). Most importantly, the increased use and growing ubiquity of alternate communications channels – IM, SMS, social networks, etc.

Stated simply, the customers of tomorrow will communicate differently than the customers of today. The customers of today are used to voice self service (although many grudgingly so). The customers of tomorrow will use new communications channels (perhaps some that do not yet exist). Your father&#8217;s customer service paradigm will probably not apply to them.

Bottom line, if you haven&#8217;t developed methods for communicating with the new wave of customers that use different modes of communication then your shiznit will be cooked. Ya dig?

<img src="http://localhost:8000/wp-content/uploads/2009/10/choices.jpg" alt="choices" title="So many choices..." style="float:left;padding:5px;margin:15px;" />

<a href="http://www.voxeo.com/" target="_blank">Voxeo</a> (the company for which RJ is CTO) has been on a buying spree of late, with the seeming goal of shoring up its offerings to cover a wide array of different communication modalities. I&#8217;ve expounded in the past on one acquisition in particular as being especially relevant – particularly as it relates to the next generation of customers – the [acquisition of IMified](http://www.voiceingov.org/blog/?p=860).

<a href="http://www.imified.com/" target="_blank">IMified</a> provides a simple API that allows developers to create applications that work across a host of IM networks, SMS and even Twitter. Voxeo has leveraged this acquisition to deploy new functionality on its core voice application platform to allow developers to deploy multimodal applications – apps that a user can interact with through several different modalities, whichever is most convenient for them.

Multimodal applications are not new – I&#8217;ve [written about them](http://www.voiceingov.org/blog/?page_id=41) many times and <a href="http://github.com/mheadd/nys-bill-bot" target="_blank">built several</a>. But Voxeo and IMified have taken the notion of multimodality to a new level by making it practical for almost any developer to build one. Even more compelling, Voxeo&#8217;s platform lets you <a href="http://blogs.voxeo.com/voxeodeveloperscorner/2009/08/27/how-to-im-and-textsms-your-voicexml-applications/" target="_blank">re-purpose applications</a> developed for one specific modality (i.e., phone) for others (SMS, or IM).

Multimodal functionality is pretty much a requirement these days for successful customer interactions, but RJ&#8217;s presentation got me thinking about other possibilities.

### Cascading Modality

The next step in the evolution of multimodal applications will be to support what I call “cascading modality.” Cascading applications will allow users to move across modalities over the course of one interaction with a company or a government.

For example, say a company wants to start a customer off in a communications channel that has relatively low cost – IM – using an application to collect basic customer information at the start of an interaction. At some point during this IM session, the customer could opt to move to a different modality. Say they send the following to the IM bot:

> #switch 6401254789

This could generate an outbound call to (640) 125-4789 so that the caller could interact with an IVR system to complete their interaction – say, if they began the IM session on their desktop computer at the office and completed it while walking to the parking garage to get in their car. The information entered during the initial IM session is persisted across the switch to the IVR call, and all of the information (from both modes) is captured in a fulfillment or CRM system.

This session could be followed up by using a third modality – perhaps a confirmation message or receipt that is sent via SMS or even e-mail.

### Concurrent Modality

Now consider another scenario, perhaps one that involves an <a href="http://www.nytimes.com/2009/10/29/technology/personaltech/29basics.html" target="_blank">older user</a> who may be less comfortable with IM. This person could send the following to an IM bot:

>   * **User**: #assist 
>   * **IM Bot**: I&#8217;d like to call you, and provide some additional assistance over the phone. Enter your 10-digit phone number.
>   * **User**: 6401254789
>   * **IM Bot**: Thank you. Hold one second while your call is placed.

As in the previous scenario, this would generate an outbound call to (640) 125-4789 but the focus of the IVR would not be to collect information – you still want the user to enter the information into the IM client they are using. The focus here would be to use the IVR to provide supportive information, so that the caller can more easily or efficiently enter required information.

One example of this &#8220;tag team&#8221; approach would be to simplify the input of information that needs to be in a particular format:

>   * **IVR App says**: Enter your account number, which is a three part number separated by dashes. Enter all leading zeros on the left hand side of your account number.
>   * **IM Bot displays**: Example: 00012345-87-1
>   * **User enters**: 00078945-44-9

By using two modalities simultaneously to interact with the user, the information can be collected in less steps – a typical IVR system would probably collect this type of an account number in three separate steps, and could be prone to error (&#8220;to the left of the first dash, or the second..?&#8221;).

In this scenario, if a user enters `` on their phone or sends `#help` via IM, they could automatically be routed to an agent for assistance.

### Building Next Generation Multimodal Apps

Companies like Voxeo have removed a lot of the complexity from building multimodal applications, but developers will need to take heed of several factors that will become important as these kinds of applications become more widespread.

_State persistence_. Cascading modality will only work if a user can switch seamlessly from one mode to another without repeating data entry. VoiceXML applications and IM bots typically communicate with a backend via HTTP, which is stateless. And while there any number of different ways to maintain state in an HTTP-based application, they do not always scale well. Things can get complicated when clusters of servers or load balancers are required. These considerations require specialized skills to address properly.

_Secure data transfer_. A profusion of multimodal applications can raise questions about data security, particularity if said data is transmitted across pubic IM and social networks. Developers need to think clearly about what is suitable for transmission across these networks, and ways that data security can be enhanced where needed.

Yes, the dawn of a new customer service era is upon us my friends. Who knows, if you tool on over to the <a href="http://evolution.voxeo.com/" target="_blank">Voxeo</a> or <a href="http://www.imified.com/" target="_blank">IMified</a> developer sites, you might just get an opportunity to help build it.