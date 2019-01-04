---
id: 1899
title: 'Building Cloud Communication Apps with Tropo: Part 3'
date: 2010-06-21T07:41:01+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1899
permalink: /building-cloud-communication-apps-with-tropo-part-3/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] Building Cloud Communication Apps with @tropo and @limonadephp: Part 3. #url# #VoIP #IM #SMS #IVR #PHP'
wp_jd_clig:
  - |
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
    <!-- turing_cluster_prod -->
    <html>
    <head>
    <title>cli.gs</title>
    <meta name="keywords" content="cli.gs">
    <meta name="description" content="cli.gs">
    <meta name="robots" content="INDEX, FOLLOW">
    <meta name="revisit-after" content="10">
    
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    </head>
    <frameset rows="100%,*" frameborder="no" border="0" framespacing="0">
    <frame src="http://www.dsnextgen.com?epl=01120090VGsLXARUBwxVAEQHVwgHWg9aB1oGCFQNCBtSSx5TXgpqRgdeXBdWRlwWFhAFVFQEVgVUBlADBEcIRmpVV1ZYCVAJUh5YBlBREUQ9BlYGCVcIWgEK" name="cli.gs">
    </frameset>
    <noframes>
    <body>
    <a href="http://www.dsnextgen.com?epl=01120090VGsLXARUBwxVAEQHVwgHWg9aB1oGCFQNCBtSSx5TXgpqRgdeXBdWRlwWFhAFVFQEVgVUBlADBEcIRmpVV1ZYCVAJUh5YBlBREUQ9BlYGCVcIWgEK">
    Click here to go to cli.gs </a>.
    </body>
    </noframes>
    </html>
wp_jd_target:
  - |
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
    <!-- turing_cluster_prod -->
    <html>
    <head>
    <title>cli.gs</title>
    <meta name="keywords" content="cli.gs">
    <meta name="description" content="cli.gs">
    <meta name="robots" content="INDEX, FOLLOW">
    <meta name="revisit-after" content="10">
    
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    </head>
    <frameset rows="100%,*" frameborder="no" border="0" framespacing="0">
    <frame src="http://www.dsnextgen.com?epl=01120090VGsLXARUBwxVAEQHVwgHWg9aB1oGCFQNCBtSSx5TXgpqRgdeXBdWRlwWFhAFVFQEVgVUBlADBEcIRmpVV1ZYCVAJUh5YBlBREUQ9BlYGCVcIWgEK" name="cli.gs">
    </frameset>
    <noframes>
    <body>
    <a href="http://www.dsnextgen.com?epl=01120090VGsLXARUBwxVAEQHVwgHWg9aB1oGCFQNCBtSSx5TXgpqRgdeXBdWRlwWFhAFVFQEVgVUBlADBEcIRmpVV1ZYCVAJUh5YBlBREUQ9BlYGCVcIWgEK">
    Click here to go to cli.gs </a>.
    </body>
    </noframes>
    </html>
original_post_id:
  - "1899"
categories:
  - Development Tools
  - Tropo
  - Tutorials
  - Twitter
  - VoIP
tags:
  - IM
  - pstv
  - SMS
  - voice
---
This post is a continuation of the series on building cloud communication applications with Tropo, the [PHP WebAPI Library](http://github.com/tropo/tropo-webapi-php) and the [Limonade framework](http://www.limonade-php.net/) for PHP .

If you&#8217;re just starting, you can take a look back at [part 1](http://www.voiceingov.org/blog/?p=1817) and [part 2](http://www.voiceingov.org/blog/?p=1882) to get caught up.

In this post, we&#8217;ll continue our work from the last post and complete a simple, yet powerful multi-channel application that can be accessed via telephone, SMS or IM client.

In the previous post, we looked closely at the [Session](https://www.tropo.com/docs/webapi/session.htm) and [Result](https://www.tropo.com/docs/webapi/result.htm) objects &#8211; these are JSON objects that are sent to your application by the Tropo platform that contain information about how a user is accessing your app (i.e., through which channel) and any input they have provided in response to prompts. If you worked through the last post, you have a partially complete script that looks like this:

You should save this script to a server that can be accessed by the Tropo platform &#8211; any web hosting platform that supports PHP >= 5.2.0 will do. Let&#8217;s call our script `get_zip_code.php`.

When you set up the start URL for this script in the Tropo Application Manager, you&#8217;ll want to structure it like so:

> http://name\_of\_my\_host.com/path/to/get\_zip_code.php?**uri=start** 

As you can see, we&#8217;ve added a querystring parameter called `uri`. This will ensure that the initial HTTP POST to this script by the Tropo platform matches our _/start_ pattern and executes our **zip_start()** method, which is where we want users to begin. Make sure you [review the Limonade documentation on setting up routes](http://www.limonade-php.net/README.htm), as there are multiple options for configuring route pattern matching.

Next, we&#8217;ll want to start modifying our partially constructed script. First go to step 6 in the **zip_start()** method, where we had set up a <a href="http://www.postbin.org/" target="_blank">PostBin</a> URL for Tropo to send a user&#8217;s input to so we could examine the Result object. Now that we know what the Result object looks like, we want to start using it to look up information and present it to the caller.

You&#8217;ll want to set up a URL to the `get_zip_code.php` script that will match the route for the **zip_end()** method. This is where we will access the Tropo Result object and process it. Change the URL in the &#8220;next&#8221; array element to look like this:

> $tropo->on(array(&#8220;event&#8221; => &#8220;continue&#8221;, &#8220;next&#8221; => &#8220;get\_zip\_code.php?**uri=end**&#8220;, &#8220;say&#8221; => &#8220;Please hold.&#8221;)); 

This change tells Tropo that when the &#8220;continue&#8221; event is raised (after the caller has completed entering input) POST the Result object back to the `get_zip_code.php` script using a relative URL and a querystring parameter that will ensure matching of our _/end_ pattern.

Next, we need to build out the **zip_end()** method to process the results:

> <pre>dispatch_post('/end', 'zip_end');
function zip_end() {

        // Step 1. Create a new instance of the result object
	$result = new Result();
	$zip = $result-&gt;getValue(); // get the value of the user input.

        // Step 2. Get weather information for the zip code the caller entered.
	$weather_info = getWeather($zip);
	$city = array_pop($weather_info);

        // Step 3. Create a new instance of the Tropo object.
	$tropo = new Tropo();

        // Step 4. Begin telling the user the weather for the city their zip code is in.
	$tropo-&gt;say("The current weather for $city is...");

        // Step 5. Iterate over an array of weather information.
	foreach ($weather_info as $info) {
	    $tropo-&gt;say("$info.");
	}

        // Step 6. Say thank you (never hurts to be polite) and end the session.
	$tropo-&gt;say("Thank you for using Tropo!");
        $tropo-&gt;hangup();

        // Step 7. Render the JSON for the Tropo WebAPI to consume.
       return $tropo-&gt;RenderJson();

}
</pre>

As you can see, our **zip_end()** method looks similar to our **zip_start()** method &#8211; both use a Tropo object to format information that will be presented to the user, and both call the **RenderJson()** method of the Tropo object at the end.

You may be wondering about the **getWeather()** method that is called in step 2. Let&#8217;s build that out now and examine how it works &#8211; to keep things simple, we&#8217;ll make use of the [Google Weather API](http://blog.programmableweb.com/2010/02/08/googles-secret-weather-api/), which provides weather information by zip code and returns the information in XML format.

> <pre>// The URL to the Google weather service. Renders as XML doc.
define("GOOGLE_WEATHER_URL", "http://www.google.com/ig/api?weather=%zip%&hl=en");

// A helper method to get weather details by zip code.
function getWeather($zip) {

	$url = str_replace("%zip", $zip, GOOGLE_WEATHER_URL);
	$weatherXML = simplexml_load_file($url);
	$city = $weatherXML-&gt;weather-&gt;forecast_information-&gt;city["data"];
	$current_conditions = $weatherXML-&gt;weather-&gt;current_conditions;
	$current_weather = array(
		"condition" =&gt; $current_conditions-&gt;condition["data"],
		"temperature" =&gt; $current_conditions-&gt;temp_f["data"]." degrees",
		"wind" =&gt; formatDirection($current_conditions-&gt;wind_condition["data"]),
		"city" =&gt; $city
	);
	return $current_weather;

}

// A helper method to format directional abbreviations.
function formatDirection($wind) {
	$abbreviated = array(" N ", " S ", " E ", " W ", " NE ", " SE ", " SW ", " NW ");
	$full_name = array(" North ", " South ", " East ", " West ", " North East ", " South East ", " South West ", " North West ");
	return str_replace($abbreviated, $full_name, str_replace("mph", "miles per hour", $wind));
}
</pre>

The mechanics of these functions are pretty straighforward, so I won&#8217;t go in to too much detail &#8211; you can now see the connection between the call to the **getWeather()** method mentioned above and the array of weather data that it returns.

The last thing we need to do in order to complete our zip code weather demo script is to finish the **zip_error()** method. This is a method we&#8217;ll use to tell a user an error occurred (never hurts to be prepared for the unexpected):

> <pre>dispatch_post('/error', 'zip_error');
function zip_error() {

	// Step 1. Create a new instance of the Tropo object.
	$tropo = new Tropo();

	// Step 2. This is the last thing the user will be told before the session ends.
	$tropo-&gt;say("Please try your request again later.");

	// Step 3. End the session.
	$tropo-&gt;hangup();

	// Step 4. Render the JSON for the Tropo WebAPI to consume.
	return $tropo-&gt;renderJSON();
}
</pre>

In order for this method to be invoked, we need to make sure that we set up the proper handler in our **zip_start()** method for it. The Tropo WebAPI makes it possible to set up callback methods that handle things when certain events are raised. This is done by [using the On object](https://www.tropo.com/docs/webapi/on.htm).

Setting up an event handler using the On object with the [PHP WebAPI Library is easy](http://github.com/tropo/tropo-webapi-php). In fact, we&#8217;ve already done it once &#8211; look at the **zip_start()** method and you&#8217;ll see a hander for the &#8220;continue&#8221; event (which is raised when a user has finished entering the proper input). We want to set up something similar for when an error event is raised. Let&#8217;s add a handler in our zip_start() method for an error event:

> <pre>// Step 6. Tell Tropo what to do when the user has entered input, or if there is an error.
	$tropo-&gt;on(array("event" =&gt; "continue", "next" =&gt; "get_zip_code.php?uri=end", "say" =&gt; "Please hold."));
	$tropo-&gt;on(array("event" =&gt; "error", "next" =&gt; "get_zip_code.php?uri=error", "say" =&gt; "An error has occured."));
</pre>
> 
> <pre></pre>

Our script is now <a href="http://gist.github.com/446713" target="_blank">complete</a> and ready to test.

Make sure you log into your [Tropo](https://www.tropo.com/) account and set up the start URL to your script as discussed above. You can test this script with the phone numbers that are automatically provisioned by Tropo when you set up your account.

Tropo will automatically provision a Skype number, a SIP number and an iNum. You can additionally add a PSTN number in a range of different area codes at no charge. This PSTN number can also be used to send an SMS to, so you can interact with this script via text message. Additionally, you can add an IM account, so you can test this script using your favorite IM client/network.

You may notice, if you test this script using SMS or IM that there are things that don&#8217;t yet work perfectly. In the next post, we will make some very simple changes to this script to optimize it for use with SMS and IM (and even Twitter!).

This will transform our simple PHP script into a powerful [unified communications](http://www.voiceingov.org/blog/?p=1314) application.

Stay tuned&#8230;