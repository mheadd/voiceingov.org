---
id: 1982
title: Make the Cloud Listen (and Understand)
date: 2010-08-13T11:11:28+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1982
permalink: /make-the-cloud-listen-and-understand/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "1982"
categories:
  - Development Tools
  - Open Source
  - Tropo
  - Tutorials
tags:
  - IM
  - limonadephp
  - PHP
  - semantic interpretation
  - speech recognition
---
Yesterday I wrote [a post about the changing cloud telephony landscape](http://www.voiceingov.org/blog/?p=1972), and highlighted some key factors that will dictate which cloud telephony providers are around for the long haul and deliver the next innovations.

One of those factors &#8211; support for speech recognition &#8211; is a good differentiator for developers to use when choosing a cloud telephony platform.

Speech recognition is becoming increasingly important in our everyday lives. Smartphones and powerful handheld devices enable multimodality, and there are [more and more restrictions](http://www.ghsa.org/html/stateinfo/laws/cellphone_laws.html) placed on our use of phones while doing other tings (like driving).

Plus, I can&#8217;t think of a more deflating concept than a cloud telephony provider that allows developers to build sophisticated apps and mashups in the language of their choice but that chains users of those apps to a telephone keypad. No fun.

To give an example of how powerful speech recognition can be, and how easy it is to use with a cloud telephony provider that supports it, I worked up a small demo to illustrate the point. The [sample code](http://gist.github.com/522913) for this demo is on Github, and we&#8217;ll dive into it in more detail below.

This demo uses two PHP libraries that are designed to work with the [Tropo](https://www.tropo.com/home.jsp) platform (one of the only cloud telephony providers to support speech recognition):

  * A set of PHP classes [for working with the Tropo WebAPI](http://github.com/tropo/tropo-webapi-php).
  * A PHP library for [creating SRGS grammars](http://github.com/mheadd/PHPGrammar) in XML format.

If you&#8217;ve read any of [my previous posts on build applications for the Tropo platform](http://www.voiceingov.org/blog/?p=1918), you&#8217;ll see lots of similarities between this and previous sample apps. Here I continue my use of the insanely awesome [Limonade Framework](http://www.limonade-php.net/) for PHP.

Let&#8217;s take the example of a company directory that allows callers to dial a single number, select a person or department at the company and then be transferred to the person they select.

With cloud telephony, there is no need to have such a system live on a machine in the server room &#8211; it can be hosted externally in the cloud, making it easier to manage and to scale. In addition, with the Tropo Platform, it doesn&#8217;t have to be the same tired old DTMF-based menu telling callers to press an extension number or to &#8220;dial by name&#8230;&#8221;.

Using the [PHP WebAPI Library](http://github.com/tropo/tropo-webapi-php) and [Limonade](http://www.limonade-php.net/), we can construct a simple, yet power script that looks like this:

This script is pretty self-explanatory, but there are some key points I want to emphasize. First, note the `$options` array that holds the reference to an external grammar file (more on that in a bit). Tropo seems to need for this reference to be an absolute one and not a relative reference to the file (not hard to do with PHP &#8211; you just need to be aware of it).

Also, the file reference needs to include a trailing parameter indicating that this is an XML grammar (`;type=application/grammar-xml`). This seems to be true even if the grammar file is served with the correct MIME type by whatever is serving it.

Now lets have a look at this grammar file.

This simplistic example demonstrates how to use the [PHPGrammar library](http://github.com/mheadd/PHPGrammar). Note the simple array structure that is being used to hold the details of employees for our fictitious company. This could very easily be replaced with a dip into a data source of pretty much any kind, like an LDAP directory or database holding employee details.

Also note in this example that we want to do something referred to as [Semantic Interpretation](http://www.w3.org/TR/semantic-interpretation/). Our grammar file is a set of rules that will be applied to what the caller says &#8211; Semantic Interpretation (SI) dictates the value that is given to our application from the grammar when a successful match occurs.

In this example, we want the caller to be able to say the name of the person they want to be transfered to. We make the first name optional so they may either say the last name of the person or (optionally) the full name. Obviously this may need to be changed based on the size of the directory to render in a grammar file (e.g., multiple employees with the same last name).

Do note that the Tropo platform seems to require the &#8220;Script&#8221; sytax for returning SI values on a successful match as opposed to the &#8220;String Literal&#8221; syntax. (More on these alternatives [here](http://www.w3.org/TR/semantic-interpretation/#SI3.2.2).)

> Works on Tropo (Script syntax):
  
> `<item>foo<tag><strong>out="bar";</strong></tag></item>`
> 
> Does not work on Tropo (String Literal syntax):
  
> `<item>foo<tag><strong>bar</strong></tag></item>` 

So, when a caller says the name of a person in our company directory we want to return the number for that person to our Tropo script so we can transfer the call to them. This can clearly be seen when we examine the Result object that is delivered by the Tropo platform.

Tropo&#8217;s Result object includes the full grammar engine output, and lots of very detailed information about the recognition. As you can see, the **utterance** that the speech recognition engine heard was the name of one of our faux employees. The **value** that was returned is the number of that person.

We use this value in the `transfer_call()` method of our Tropo script.

> `<br />
	// Create a new instance of the Result object.<br />
	$result = new Result();</p>
<p>        // Get the value of the selection the caller made.<br />
	$phone = $result->getValue();</p>
<p>	// Create a new instance of the Tropo object and transfer the call.<br />
	$tropo = new Tropo();<br />
	$tropo->transfer('+1'.$phone);</p>
<p>	// Write out the JSON for Tropo to consume.<br />
	$tropo->RenderJson();<br />
` 

Using the PHP WebAPI library, it takes just 5 lines of code (excluding comments) to get the value of the grammar result and transfer the call. How cool is that?!

Obviously there are lots of things that can be done to enhance this script, to make it more robust, but it illustrates the essential concepts of speech recognition in the cloud.

What&#8217;s more, because of all of the great functionality provided by the [Tropo cloud platform](https://www.tropo.com/home.jsp) we can really push the envelope on the tired old company directory:

  * We could take an inbound call from a Skype user and transfer to a cell phone (or a SIP endpoint).
  * We could let our caller select a department in our company and then ring several different numbers at once, transferring the call to the first one answered (sort of a &#8220;hunt group in the cloud&#8221;).
  * We could use Tropo&#8217;s built in IM capabilities to send a screen pop to the person receiving the call.

The sky is the limit. Which I guess is the point of cloud telephony&#8230;