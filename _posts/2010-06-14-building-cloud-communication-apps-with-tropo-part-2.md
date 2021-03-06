---
id: 1882
title: 'Building Cloud Communication Apps with Tropo: Part 2'
date: 2010-06-14T18:30:20+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1882
permalink: /building-cloud-communication-apps-with-tropo-part-2/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] Building Cloud Communication Apps with @Tropo: Part 2. #url# #VoIP #SMS #IM #IVR'
wp_jd_clig:
  - http://cli.gs/QUQSa
wp_jd_target:
  - http://www.voiceingov.org/blog/?p=1882
original_post_id:
  - "1882"
categories:
  - Development Tools
  - Tropo
  - Tutorials
  - Twitter
  - VoIP
tags:
  - limonadephp
---
This post is a continuation of the series on building cloud communication applications with Tropo and the [PHP WebAPI Library](http://github.com/tropo/tropo-webapi-php).

In this post, we&#8217;ll be looking at Tropo&#8217;s support for multi-channel applications and using the incredibly flexible and powerful [Limonade](http://www.limonade-php.net/) library for PHP (think [Sinatra](http://www.sinatrarb.com/) for PHP).

**Working with the Session Object**

As I explained very briefly in the [previous post on this subject](http://www.voiceingov.org/blog/?p=1817), the Tropo WebAPI is an HTTP/JSON API for building multi-channel communication apps.

What this means essentially is that the Tropo platform does all of the hard stuff involved with executing a communication app &#8211; DTMF/speech recognition, rendering Text-To-Speech (TTS), maintaining and managing all of the connections to the different communication networks (PSTN, SMS, IM networks, Twitter). You tell Tropo how to govern the interaction between a caller and your application on a specific channel by sending it a set of instructions in [JSON](http://www.json.org/) format.

In this series of posts, we&#8217;re using the [PHP WebAPI Library for Tropo](http://github.com/mheadd/TropoPH) to generate the JSON that gets sent to, and consumed by Tropo. But this exchange of JSON isn&#8217;t one-way &#8211; Tropo also sends JSON packages to your application with important information about (among other things) the network a user selects to interact with your application on and any input they have provided in response to prompts.

At the beginning of a user session (when a user first connects to your application), Tropo will deliver a JSON [Session](https://www.tropo.com/docs/webapi/session.htm) object to your application. This object contains all sorts of useful information that your app can use when rendering out JSON instructions to send back to Tropo. Let&#8217;s examine what a real life Session object looks like.

The easiest way to do this is to simply go over to <a href="http://www.PostBin.org" target="_blank">PostBin.org</a> and make a new PostBin. PostBin is a service that lets you see HTTP posts that get sent to the special URL that is generated when you create a new PostBin.

After you have created a new PostBin, log into your [Tropo account](https://www.tropo.com) and create a new WebAPI application. Use the PostBin URL as the URL that powers your new Tropo WebAPI app. After your app is created, you will have a newly provisioned Skype number that you can use to call it.

When you call your application using the Skype number provisioned by Tropo, you won&#8217;t hear anything &#8211; remember, we haven&#8217;t yet generated any JSON to tell the Tropo platform what to say or do when a user connects. After you make your call (it will be over quickly), go back to your PostBin URL (you may need to refresh) and you will see an object in JSON format, like this:

This is the Session object for the call you just made. It&#8217;s what is sent to your application (via HTTP POST) each time a new session is started on Tropo. Working with this object using the PHP WebAPI Library is easy. You just create a new instance of the Session object in PHP and you can start accessing the properties of this object:

> `<br />
$session = new Session();<br />
$from_info = $session->getFrom();<br />
echo $from_info['channel'];</p>
<p>// Using the example Session object JSON from above would render VOICE.<br />
` 

Being able to access the channel and network a user is accessing your application from can be useful when you want to tailor prompts or actions to a specific channel &#8211; e.g., a phone call vs. an IM session.

Also make note of the _initialText_ property &#8211; this will be important when building SMS and IM applications, where a user will begin an interaction with your application by sending information to it. This property will allow you to process the initial input for those channels without having to ask the user for it again (something users generally dislike).

Next, let&#8217;s take a look a the Result object that is sent from Tropo to your application when a user provides input in response to a prompt or direction. In order to do this, we need to take a sip of Limonade.

**Mmmm&#8230; Limonade!**

[Limonade](http://www.limonade-php.net/) is a lightweight PHP framework that is very much like the Sinatra framework for Ruby. I won&#8217;t go into too much detail on it, as there is ample documentation available on the [Limonade site](http://www.limonade-php.net/) , but here is quick introduction that will let us build enough of a structure to see the Tropo result object.

When you use Limonade, you set up routes for HTTP requests. A route is comprised of an HTTP method, a URL matching pattern and a PHP method. When an HTTP request is made to a URL that matches the pattern, and uses the method specified in the route, the designated PHP function gets invoked. For example:

> <pre>dispatch_post('/', 'test');
  function test() {
    echo 'This is a test.';
}
</pre>

The &#8216;dispatch_post()&#8217; directive specifies that the HTTP method for this route with be POST (which is what is used by Tropo to send JSON to your application). The two parameters to this directive specify the URL pattern to match (in this case, the root directory on the domain were this script is located) and the PHP method to invoke, which is defined below this directive. In a nutshell, whenever an HTTP POST is made to the root domain where this script is located, the text _This is a test_ will be rendered.

Let&#8217;s build out a simple shell that we&#8217;ll use to construct our Tropo application for the next few posts in this series:

> <pre>// Include Tropo classes.
require('TropoClasses.php');

// Include Limonade framework (http://www.limonade-php.net/).
require('path/to/limonade/lib/limonade.php');

dispatch_post('/start', 'zip_start');
function zip_start() {
	// Tell the user to enter their zip code.
}

dispatch_post('/end', 'zip_end');
function zip_end() {
	// Do something with the entered zip code.
}

dispatch_post('/error', 'zip_error');
function zip_error() {
	// Tell the user an error has occurred.
}

// Run this sucker!
run();

</pre>

Our Tropo application will collect a user&#8217;s zip code and then look up some information based on the input they provide. As you can see, we&#8217;ve included the PHP WebAPI Library and the Limonade Framework. We&#8217;ve also set up three Limonade routes _start_, _end_ and _error_ (all using the HTTP POST method) and stubbed out the PHP function that will render JSON for Tropo to consume.

To get a look at the Tropo Result object, lets add some logic to the **zip_start()** function:

> <pre>dispatch_post('/start', 'zip_start');
function zip_start() {

	// Step 1. Create a new instance of the Session object, and get the channel information.
	$session = new Session();
	$from_info = $session-&gt;getFrom();
	$network = $from_info['channel'];

       // Step 2. Create a new instance of the Tropo object.
	$tropo = new Tropo();

	// Step 3. Welcome prompt.
	$tropo-&gt;say("Welcome to the Tropo PHP zip code example for $network");

	// Step 4. Set up options for zip code input.
	$options = array("attempts" =&gt; 3, "bargein" =&gt; true, "choices" =&gt; "[5 DIGITS]", "name" =&gt; "zip", "timeout" =&gt; 5);

	// Step 5. Ask the caller for input, pass in options.
	$tropo-&gt;ask("Please enter your 5 digit zip code.", $options);

	// Step 6. Tell Tropo what to do when the user has entered input. Enter your PostBin URL in the "next" array element.
	$tropo-&gt;on(array("event" =&gt; "continue", "next" =&gt; "http://www.PostBin.org/xxxxxxx", "say" =&gt; "Please hold."));

	// Step 7. Render the JSON for the Tropo WebAPI to consume.
	return $tropo-&gt;RenderJson();

}

</pre>

As you can see, inside this function we create a new instance of the Session object and get the channel the user is accessing our application from. We also create a new instance of the Tropo object (this is what we&#8217;ll use to send JSON instructions back to the Tropo platform).

The next several steps are fairly self explanatory, but take special note of Step 6. Here we are telling the Tropo platform that when a &#8216;continue&#8217; event is raised (when a user finishes entering input) tell them to &#8216;Please hold&#8217; and then POST the results of their input to a PostBin URL. (Note &#8211; replace the value above with the PostBin URL you used at the beginning of this tutorial.)

**Working with the Result Object**

Save your script and change the URL for your WebAPI application in the Tropo Applications manager to point to it. You can now test your script using the the Skype number for your app as we did before . When you access your script, you&#8217;ll get the instructions to enter a zip code, after which Tropo will POST the results to the PostBin URL you inserted into the script in Step 6 above.

Now, when you look at your PostBin URL, you&#8217;ll see something like this:

As you can see, the Result object that gets sent from Tropo to your app has a wealth of information on what the user entered, how it was interpreted by Tropo and even the confidence level of the recognition (if speech recognition is used).

You can access the Result object using the PHP WebAPI Library just like you can the Session object:

> `</p>
<p>$result = new Result();<br />
$zip = $result->getValue();<br />
echo $zip</p>
<p>// Using the example Result object JSON from above would render 12345<br />
` 

You would use the Result object in the **zip_end()** function we stubbed out above. You use the value of the zip code entered to look up information relevant for that zip code (like a weather forecast) and present it to the caller.

In the next post in this series, we&#8217;ll complete our simple zip code example by adding a weather forecast lookup and present it to the user. We&#8217;ll also tweak our script to optimize it for different channels that a user might employ to access it, to ensure the experience is optimized for phone, IM and SMS.

Stay tuned&#8230;