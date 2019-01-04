---
id: 1398
title: User Agent Sniffing for Multi-Channel Apps
date: 2009-12-05T19:46:06+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1398
permalink: /user-agent-sniffing-for-multi-channel-apps/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] User Agent Sniffing for Multi-Channel Apps. #url# #voxeo #ivr #sms'
original_post_id:
  - "1398"
categories:
  - Development Tools
  - Tutorials
tags:
  - IM
  - SMS
  - VoiceXML
  - Voxeo
---
Several months ago, voice platform company Voxeo announced an exciting addition to its <a href="http://www.voxeo.com/prophecy/" target="_blank">industry leading VoiceXML platform</a> &#8211; the ability to repurpose existing VoiceXML applications and turn them into instant messaging (IM) applications and text messaging (SMS) applications.

And while I&#8217;ve had several opportunities to expound on the importance of this announcement, I have not yet had time to use a practical example to demonstrate just how powerful the new functionality Voxeo is now offering can be.

### How big a deal is this really?

To be honest, when I first read about Voxeo&#8217;s new offering, I didn&#8217;t think it was a big deal. I thought it was a _freaking huge_ deal &#8211; I&#8217;m talking Godzilla, baby. This is big!

Lots of people are wary of proprietary extensions to open standards like VoiceXML because they can make your code less portable (if you find you need to transition from one vendor to another). But unlike other platform-specific extensions to VoiceXML, Voxeo&#8217;s new feature doesn&#8217;t make you write VoiceXML in a way that violates the standard, it changes the way that the platform consumes VoiceXML.

The example that I will demonstrate below is written in 100% standard-compliant VoiceXML. It can be run on any VoiceXML platform that adheres to the VoiceXML spec, and probably a bunch that don&#8217;t.

That&#8217;s why this announcement from Voxeo is a game changer for developers &#8211; it provides sophisticated and powerful new functionality without the burden of having to alter your code in a way that makes it less portable. In addition, it allows developer to create sophisticated applications that can be delivered to users via IM and SMS using all the familiar tools of VoiceXML.

All of the benefits of working with VoiceXML are available for developers that want to write SMS and IM apps &#8212; grammars, the <acronym title="Form Interpretation Algorithm">FIA</acronym>, the standard <a href="http://www.w3.org/TR/2004/REC-voicexml20-20040316/#dml1.5" target="_blank">root-leaf structure</a> of VoiceXML applications &#8212; its all there.

### The nose knows

While it might be tempting to think that **any** existing VoiceXML application can be repurposed with no changes for IM and/or SMS, this is probably not true except for the most simplistic of apps. Just as there are certain types of interactions that may make sense for use with visual web applications and not with voice apps, there are certain types of dialog flows that work well in voice apps but don&#8217;t necessarily translate well to the world of IM and SMS.

Consider the following:

<li style="margin-bottom:4px;">
  Phone applications often employ a confirmation dialog, to prompt a caller to verify what they have just entered &#8211; particularly if the information is important or sensitive (e.g., a credit card number, or account number). This probably not a good practice with SMS or IM apps because, unlike with a phone application, a user can see what information they are about to submit <em>before</em> the actually send it. Also, since multiple text messages can mean increased changes for a user its a good idea to cut down on this where possible.
</li>
<li style="margin-bottom:4px;">
  DTMF-based phone applications often use prompts like &#8220;Press 1 to repeat, press 2 to go to the previous menu&#8230;&#8221; The notion of &#8220;pressing&#8221; something is really tied to the telephone key pad and is probably not appropriate for SMS or IM applications. Its much clearer to use prompts like &#8220;Enter 1 to go to the next option&#8221;, or &#8220;Send #back to go to the previous step&#8221;.
</li>
  * Re-prompting a user on noinput is a pretty standard practice in phone applications, but is probably not a good idea in IM or SMS apps. If a person is using their cell phone to interact with an SMS application, and then gets a call, they probably do not want to be repeatedly prompted for input by your app that they will not have an opportunity to enter until after their call has finished. 

So if all of this is true, then how does a developer determine how an application is being accessed? It seems pretty clear that there needs to be a way to determine if an application is being called from a phone, from an SMS cell phone or from an IM client.

Turns out, there is &#8211; good old fashioned browser sniffing.

### A simple example

The <a href="http://blogs.voxeo.com/voxeodeveloperscorner/2009/08/27/how-to-im-and-textsms-your-voicexml-applications/" target="_blank">Voxeo blog</a> has a good overview of this new feature, and how you can set up and run a VoiceXML application that is also IM and SMS enabled.

One of things that is really nice about this new feature of the Voxeo Prophecy platform is that the text that is sent to a user via SMS or IM, and all of the inputs sent back from the user, follow the same logic rules as with a phone interaction. This means that if there are VoiceXML elements with conditional attributes on them, they get evaluated when rendering text for SMS and IM, just like they do for the telephone.

Another really nice feature is that Voxeo lets you deploy a single number for SMS and traditional phone access to your application. So if you dial a provisioned number on your telephone, the traditional VoiceXML browser engages and executes your code in the typical fashion. If you send an SMS message to the number, the new Prophecy 10 browser engages your code and manages the SMS interaction.

Because of this, its fairly straightforward to detect which browser is accessing your code, and create a simple variable declaration that will govern how your output is rendered.

The following is a simple PHP class that can be used to sniff the browser type requesting a specific file.

To use this class, we simply include it in our PHP page that will render VoiceXML, determine what kind of user agent is requesting the page by calling `getChannelType()` and set a VoiceXML variable accordingly.

If an SMS or IM client is interacting with our application, the **Prophecy 10** browser will make the request. If its a standard telephone, it will be the **Prophecy 8** browser, so we really just need to use the value of `$_SERVER['HTTP_USER_AGENT']` to guide our app.

You&#8217;ll notice that if an SMS or IM client is accessing our app, we skip the confirmation field. We&#8217;ve also customized the reprompt logic on noinput to ensure that a user does not get successive SMS or IM messages telling them &#8220;Sorry, I did not get your response.&#8221;

So with a few lines of server side code, we&#8217;ve custom tailored our VoiceXML dialog to ensure that it renders properly regardless of the type of user agent accessing it.

Voxeo Prophecy&#8217;s new features are powerful, and VoiceXML developers should take notice. With Prophecy, they can leverage their skills and become crackerjack SMS and IM app developers as well.