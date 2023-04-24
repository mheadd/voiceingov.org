---
id: 135
title: 'Earth Day Special Project: GreenPhone'
date: 2008-04-17T12:15:16+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=135
permalink: /earth-day-special-project-greenphone/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "135"
categories:
  - Tutorials
---
[Earth Day 2008](https://en.wikipedia.org/wiki/Earth_Day) is fast approaching, so I wanted to try and build something that would help the environment and also be a cool demonstration of telephone applications generally, and the [Voxeo Prophecy](https://evolution.voxeo.com/) platform in particular.

I decided to whip up a simple application that would allow a caller to search for E85 and Bio-diesel fuel stations in their state. Some of the specific goals that I had in mind when I got started were:

  * To make use of the Voxeo Prophecy platform, the premiere VoiceXML/CCXML platform for building voice applications (at least in my opinion).
  * To code the application entirely in VoiceXML, CCXML, ECMAScript and PHP (that's right, no database!). 
  * To integrate with SOAP-based web services to obtain data on E85 and Bi-Diesel station locations, and to do other cool stuff like send an SMS message from VoiceXML.
  * To make use of interesting and unique audio files for prompts and to signal specific types of outcomes.

The fruits of one weekend of labor can be downloaded here (sorry, the source code for this app is not available anymore) . To set up and test this application, you will need the following:

  * An account with StrikeIron to use the web services that drive the GreenPhone application.
  * A copy of Voxeo Prophecy.
  * A good headset and microphone (to place test calls using Prophecy).
  * A cell phone (preferably one with a liberal text messaging contract).

**Sign Up With StrikeIron:**

Create an account with [StrikeIron](https://en.wikipedia.org/wiki/Strikeiron) and sign up for the Super Data Pack Web Service. This is a collection of web services that allow for up to 10,000 hits / month at no charge (where are you going to get a better deal than that?). You'll also want to sign up for the Global SMS Pro Web Service &#8211; this is the service that is used to send SMS messages from the GreenPhone application. Note &#8211; this service is priced quite differently than the Super Data Pack Web Service &#8211; only 10 free hits before you start paying. If you want to use this service for anything more than just testing out how to send an SMS message from Voxeo Prophecy, you'll need to get your wallet out.

Make note of the user ID (email address) and password used to create your StrikeIron account &#8211; these will be needed momentarily.

**Download and install Voxeo Prophecy:**

Download and install the [Voxeo Prophecy](https://evolution.voxeo.com/) software. Follow all of the instructions for installing and obtaining a license &#8211; a two-port license (which will support 2 concurrent phone calls) is free. Right now, prophecy only runs on Windows, but a Linux version is in the pipeline.

**Download and Configure GreenPhone:**

Download the [GreenPhone application](https://voiceingov.org/tutorials) and extract it to a new directory under c:{Prophecy install path}www. (For example, on my Windows machine I&#8217;ve extracted to c:Program FilesVoxeowwwGreenPhone). You don&#8217;t have to run the GreenPhone application on the same machine as Prophecy &#8211; if you decide to deploy it on another machine, it must support PHP 5 &#8211; GreenPhone makes use of the PHP SOAP and SimpleXML extensions.

Once this is complete, navigate to the directory where you just extracted the GreenPhone application files. Go to the directory called &#8220;php&#8221;, and open the file called _common.php_. At the top of this file, enter the credentials from your StrikeIron account. Save and close the file.

**Creating a Call Route for GreenPhone:**

Open the Prophecy Management Console in your web browser (http://127.0.0.1:9995/mc.php) &#8211; the default user ID and password are _admin_/_admin_. Click on the &#8220;Call Routing&#8221; option on the left hand menu &#8211; this is where you will set up a call route to the GreenPhone application.

Pick one of the numbered route Ids (e.g., Route 1 ID) and make the following changes:

  * Change the route ID to _green_ 
  * Change the Route Type to _CCXML W3C_
  * Change the URL to _http://127.0.0.1:9990/{ GreenPhone Install Directory}/greenPhoneStart.xml_
  * Scroll to the bottom of the page and click &#8220;Save Changes&#8221;

**Making a test call:**

Now that Prophecy is installed, fire up the SIP Phone that it is bundled with &#8211; you should see the Prophecy icon in your system tray. Click on it, and select &#8220;SIP Phone&#8221; from the menu. When the SIP Phone launches, select _Options_. In the _SIP Proxy / Registrar Options_ section, enter your cell phone number in the _Local Username_ field (e.g., 2125551234). Click OK, and restart your SIP Phone. This last step allows your cell phone number to be delivered as the caller ID (or ANI) on the test call you are about the make, even though your initiating the call from a SIP phone.

GreenPhone is built to use ANI to look up E85 and Bio-Diesel stations in the caller&#8217;s home state. We do this by invoking the U.S. Area Code Information Web Service that is part of the StrikeIron Super Data Pack to determine which state a caller is calling from. There are additional web services in the StrikeIron Super Data Pack that we can invoke to locate Bio-Diesel stations and E85 Stations &#8212; the methods invoked on these last two services require us to identify the state we want a listing of stations for.

The caller&#8217;s ANI is also used to send the details on a particular E85 or Bio-Diesel station via text message to the caller&#8217;s phone &#8211; so if you enter your cell phone number in the Voxeo SIP Phone as described above, you can get details on a station that may be near you sent directly to your cell phone.

As an aside, you&#8217;ll notice that a single phone call can result in up to 4 web service invocations &#8212; not really sure if that&#8217;s &#8220;too many&#8221; but there are probably some opportunities for caching that I&#8217;ll be discussing in the next couple of posts on this, as I describe in more detail how to interact with web services via Voxeo Prophecy.

Now you are ready to place a test call. When your SIP Phone restarts, go to the field called _Dial String_ and enter &#8220;sip:green@127.0.0.1&#8221; (without the quotes). Click dial and you are now interacting with the GreenPhone application!

You&#8217;ll notice (and hopefully enjoy) the unique sounds I&#8217;ve tried to used throughout the application. All of them were obtained from the [FreeSound Project](https://en.wikipedia.org/wiki/Freesound) and modified to conform to the Prophecy standard for audio files with [Audacity](http://audacity.sourceforge.net/).

There are some obvious limitations to how this application currently works, and the VUI clearly needs some refinement (DTMF only at this point).

In the next several posts, I&#8217;ll point to this application to discuss examples on how to accomplish things in VoiceXML and CCXML using the Voxeo Prophecy Platform.

Have a happy Earth Day on 4/22!!
