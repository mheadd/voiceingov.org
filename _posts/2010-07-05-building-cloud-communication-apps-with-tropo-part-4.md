---
id: 1918
title: 'Building Cloud Communication Apps with Tropo: Part 4'
date: 2010-07-05T17:18:31+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1918
permalink: /building-cloud-communication-apps-with-tropo-part-4/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] Turn a Phone Application into an #SMS #IM or #Twitter App with @Tropo and #PHP. #url#'
wp_jd_clig:
  - http://cli.gs/r5z29
wp_jd_target:
  - http://cli.gs/r5z29
original_post_id:
  - "1918"
categories:
  - Development Tools
  - Tropo
  - Tutorials
  - Twitter
---
This post is the final installment in the series on building cloud communication applications with Tropo, the [PHP WebAPI Library](http://github.com/tropo/tropo-webapi-php) and the [Limonade framework](http://www.limonade-php.net/) for PHP.

If you&#8217;re just starting, you can take a look back at [part 1](http://www.voiceingov.org/blog/?p=1817), [part 2](http://www.voiceingov.org/blog/?p=1882) and [part 3](http://www.voiceingov.org/blog/?p=1899) to get caught up.

In the last installment, we finished our <a href="http://gist.github.com/446713" target="_blank">first complete script</a> using the [PHP WebAPI Library](http://github.com/tropo/tropo-webapi-php) and Limonade. We tested this script by calling it using one of the numbers automatically provisioned for applications on the Tropo platform. In this case, we used the auto provisioned Skype number to make test calls.

In this post we&#8217;ll refine our script by optimizing it so that the exact same code can efficiently service users on an array of different communication channels. This is the definition of &#8220;multi-modality,&#8221; and the Tropo platform does it better than pretty much any other platform currently available to developers.

**So Many Channels, So Little Code**

Tropo&#8217;s strong suit is empowering developers to build applications that work across multiple channels from the same code base. Enabling different channels for existing application is easy &#8211; log into your [Tropo](http://www.tropo.com) account and go to &#8220;Your Applications.&#8221; Select the application we&#8217;ve been using for this series, and make note of the sections entitled &#8220;Phone Numbers&#8221; and &#8220;Instant Messaging Networks.&#8221;

To SMS-enable your application (so that users can simply send a text message to get weather information) select _Add a New Phone Number_. The phone number you add can be used for both voice phone calls and for SMS messages. Under &#8220;Instant Messaging Networks&#8221; you can add any one of the many networks supported by Tropo &#8211; for this example, we&#8217;ll add a Jabber account so we can test a channel other than voice.

If you set up a Jabber account for your app, you can interact with it by sending it a message &#8211; try starting things off by sending a simple message like &#8220;Hello.&#8221; Once you do, you&#8217;ll see the same series of prompts that you can hear when you call into your application via Skype.

<img src="http://localhost:8000/wp-content/uploads/2010/07/before.png" alt="IM Bot Before Changes" title="IM Bot Before Changes" width="487" height="353" style="margin:5px;padding:7px;" />

Now that we can see how our application behaves when we interact with it using an IM client, it becomes obvious that there are some things we&#8217;d like to change to optimize it for this channel. User interface elements like a welcome message, reprompts (playing a prompt over again when a user has not entered any input), etc. don&#8217;t really make much sense in the context of an IM session. More importantly, it would be nice if we could simply send a zip code to our application to begin the session, as opposed to sending a message like &#8220;Hello.&#8221;

Fortunately, Tropo was built with multi-modality baked into it so changes like these are rather trivial. To illustrate how to optimize our application so that it can be used on multiple channels, consider the Session object we examined in detail in one of the previous posts in this series.

That Session object was created when we placed a phone call to our application &#8211; note that the property name _initialText_ is **null**.

When we access our application using an IM client, the Session object looks like this:

This Session objects looks considerably different than the one created when we made a phone call to our application. In particular, you can see that the _initialText_ property is now populated with the text we first sent &#8211; the string &#8220;Hello.&#8221;

We can access this property using the [PHP WebAPI Library](http://github.com/tropo/tropo-webapi-php) like so:

> <pre>$session = new Session();
$initial_text = $session-&gt;getInitialText();
</pre>

After accessing the value of this property, we need to do something with it:

> <pre>if(strlen($initial_text) == 5 && is_numeric($initial_text)) {
	// Since the user submitted a zip code, look up weather info.
}
</pre>

Now that we can access the _initialText_ sent to our application, and we can examine it to determine if the user has sent us a valid zip code. This allows us to tailor the behavior of our app more efficiently to an IM channel _without changing how it behaves when a caller makes a telephone call to it_.

<img src="http://localhost:8000/wp-content/uploads/2010/07/after.png" alt="IM Bot Before Changes" title="after" width="484" height="196" style="margin:5px;padding:7px;" />

The modified script with changes to optimize it for IM <a href="http://gist.github.com/464579" target="_blank">can be found here</a>.

**Conclusion**

Clearly there are lots of other things we could do to tweak our application, to tailor it more efficiently to different channels supported by Tropo. For example, breaking out the weather information into discreet segments for temperature, wind, etc. (by using a separate [Say object](https://www.tropo.com/docs/webapi/say.htm) for each) might work well with a voice or IM channel, but it would probably not work well for SMS or Twitter.

Additional changes to optimize this script for these other channels is pretty straightforward. I won&#8217;t get into it in this post, but now that you&#8217;ve got the hang of how easy it is to create multi-modal communication apps with Tropo, Limonade and the [PHP WebAPI Library](http://github.com/tropo/tropo-webapi-php) you should give it a try.

Rock on!