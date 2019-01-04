---
id: 139
title: Accessing Web Services From VoiceXML
date: 2008-05-06T18:21:03+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=139
permalink: /accessing-web-services-from-voicexml/
original_post_id:
  - "139"
categories:
  - Tutorials
tags:
  - PHP
  - SOAP
  - VoiceXML
  - Web Services
---
A few weeks ago, I posted about [accessing web services from CCXML](http://www.voiceingov.org/blog/?p=136) using PHP. This post will demonstrate how to do the same thing, only from VoiceXML. We&#8217;ll be using [Voxeo Prophecy](http://voxeo.com/prophecy/) and PHP for this example. We&#8217;ll also be referring to the [GreenPhone project](http://www.voiceingov.org/blog/?p=135) &#8212; available free for download &#8212; for the sample code.

Before we dive in, its important to keep in mind that there are a number of different techniques for getting information from web services into a VoiceXML dialog. This is just one method &#8212; there are many others. Voxeo even has its own [platform-specific way of accessing SOAP web services via JavaScript](http://www.voicexmlguide.com/appendixo.htm). Ultimately, the method you employ needs to be a good fit for the environment your working in and the requirements of your project.

**Using the greenSoapClient Class**

In the last post on this topic, I demonstrated how to use a simple PHP class as a way to access multiple SOAP-based web services from CCXML. This class forms the basis of our method for accessing web services from VoiceXML as well. However, in this instance, instead of using the CCXML <send/> element, we&#8217;ll use a VoiceXML [subdialog](http://www.w3.org/TR/2004/REC-voicexml20-20040316/#dml2.3.4).

Subdialogs in VoiceXML are typically used to create reusable dialog components for capturing common types of input, like a series of digits (e.g., credit card numbers, account numbers, etc). They can also be used to compartmentalize complex interactions with a caller and provide a simple interface for accessing results. By way of example, this is how the [OSDMs from Nuance](http://www.nuance.com/dialogmodules/advanced/) work, as well as the [Targus service](http://community.voxeo.com/services_pro/targus/home.jsp) from Voxeo. We&#8217;ll borrow this approach to access a [web service from StrikeIron](http://www.strikeiron.com/ProductDetail.aspx?p=190) that will send the details of an E85 or bio-diesel station to a cell phone via SMS.

**Setting up our Subdialog**

In order to send an SMS message with details on an E85 or bio-diesel station, we&#8217;ll need 2 things; the station details, and a cell phone number to send it to.

In order to send the details on a station from VoiceXML to PHP, we&#8217;ll pack it up in a pipe-delimited string called &#8220;detailsToSend&#8221; (I won&#8217;t go into too much detail about how this is done in this post &#8212; to learn more, refer to the GreenPhone Project code). The cell phone number we are sending to is obtained from the caller ID of the calling party, stored in a variable named &#8220;ani&#8221;. Details on how to access caller ID are given in a [previous post](http://www.voiceingov.org/blog/?p=136).

Our subdialog call will look like this:
  
`<br />
<form id="sendDetails"><br />
&nbsp;<catch event="error.badfetch"><br />
&nbsp;&nbsp;<prompt><br />
&nbsp;&nbsp;&nbsp;There was a problem sending the station details to your phone.<br />
&nbsp;&nbsp;&nbsp;<break strength="weak"/><br />
&nbsp;&nbsp;</prompt><br />
&nbsp;&nbsp;<goto next="#goodbye"/><br />
&nbsp;</catch><br />
<subdialog name="sendSMS" src="../php/sendStationDetails.php" namelist="ani detailsToSend"><br />
&nbsp;<prompt><br />
&nbsp;&nbsp;Sending the station details to<br />
&nbsp;&nbsp;<say-as interpret-as="telephone"><value expr="ani"/></say-as><br />
&nbsp;</prompt><br />
&nbsp;<filled><br />
&nbsp;&nbsp;<if cond="sendSMS.result==0"><br />
&nbsp;&nbsp;&nbsp;<prompt>Your message has been sent.<break strength="weak"/></prompt><br />
&nbsp;&nbsp;<else/><br />
&nbsp;&nbsp;&nbsp;<prompt><br />
&nbsp;&nbsp;&nbsp;&nbsp;There was a problem sending the station details to your phone.<br />
&nbsp;&nbsp;&nbsp;&nbsp;<break strength="weak"/><br />
&nbsp;&nbsp;&nbsp;</prompt><br />
&nbsp;&nbsp;</if><br />
&nbsp;<goto next="#goodbye"/><br />
&nbsp;</filled><br />
</subdialog><br />
</form><br />
` 
  
We use the attributes on the <subdialog> element to give our subdialog a name (which we&#8217;ll use to access the results sent back from PHP), to specify where to POST our variables to and also to specify which variables to POST.

You&#8217;ll also notice that we have set up a handler here for an &#8220;error.badfetch&#8221; event. This is a good habit to get into whenever you set up a request to an external resource (like a PHP script). If the script isn&#8217;t there or has problems, an &#8220;error.badfetch&#8221; event will get returned and unless you specified a handler for this event, your day will not end well.

Additionally, we&#8217;ve set up logic in our filled block to inspect the result of the subdialog call. We access the result as a property of the subdialog, using the name we set up in the <subdialog> element and the dot notation (&#8220;.&#8221;) familiar to JavaScript.

<if cond=&#8221;sendSMS.result==0&#8243;>

&#8230; code logic goes here &#8230;

</if>

With this in mind, our PHP script needs to send back a variable called &#8220;result&#8221;. How do we do this? Lets take a look at the PHP script:

**A Simple Subdialog using PHP**

The subdialog that we want to render is extremely simple &#8212; we only need to render enough VoiceXML to declare a variable called &#8220;result&#8221; and return it to the parent dialog. We&#8217;ll do this after we make our web service call to send the SMS message.

There are two pieces of information returned from the StrikeIron web service that we are interested in; a string that holds the response message from the service (i.e., &#8220;success&#8221;, &#8220;failure&#8221;, etc.) and a number indicating the outcome of the web service call.

We&#8217;ll take these two bits if information and assign them to PHP variables:

`<br />
$result = $xml->soapHeader->ResponseInfo->ResponseCode;<br />
$message = $xml->soapHeader->ResponseInfo->Response;<br />
` 
  
Now, we want to write out these variables in a simple VoiceXML subdialog:

`<br />
<?xml version="1.0" encoding="utf-8"?><br />
<vxml version="2.1" xmlns="http://www.w3.org/2001/vxml"><br />
<form id="F_1"><br />
&nbsp;<log>*** SMS response message was: <?php echo $message; ?>. ***</log><br />
&nbsp;<block><br />
&nbsp;&nbsp;<var name="result" expr="<?php echo $result ?>"/><br />
&nbsp;&nbsp;<return namelist="result"/><br />
&nbsp;</block><br />
</form><br />
</vxml>`

As discussed above, this creates just enough VoiceXML to instantiate a variable and return it to the parent dialog. For good measure, we&#8217;ll write out the web service string (contained in the PHP variable $message) as a log statement, in case it contains information we want to look at later.

**Why This Approach?**

Using this technique for accessing web services from VoiceXML provides a couple of advantages. First, it allows us to completely separate the presentation layer (the VoiceXML) from the logic used to invoke the web service. This is a fairly standard design practice that makes creating the dialog much easier for a developer that does not necessarily know a whole lot about web services. With this approach, they don&#8217;t really need to &#8212; they only need to know that the subdialog call will return a variable called &#8220;result&#8221; whose value can be inspected to determine what to do next.

Additionally, because the parent dialog is just static VoiceXML it may be possible to cache it. Since the parent dialog isn&#8217;t dynamic, it can be cached for fast access, while the subdialog &#8212; which must be dynamic &#8212; is the only component sent from the web server to the VoiceXML platform each time a caller accesses the application. Careful design can yield additional caching opportunities that can make your applications more efficient and less bandwidth intensive.

In the next post, we&#8217;ll explore one additional method for accessing web service from VoiceXML. Stay tuned&#8230;