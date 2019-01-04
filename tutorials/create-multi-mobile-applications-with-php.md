---
id: 41
title: Create Multi-Mobile Applications with PHP
date: 2005-07-16T13:44:48+00:00
author: mheadd
layout: page
guid: http://www.voiceingov.org/blog/?page_id=41
original_post_id:
  - "41"
---
**What is a Multi-Mobile Application?**

The typical job description in the technology industry doesn&#8217;t include hazardous duty pay for all of the annoying acronyms, buzzwords and overhyped slogans that get tossed around. As such, I&#8217;m always cautious about using a new one (yes, I still feel a little funny talking about AJAX). However, I have noticed a trend toward a new type of web application that is focused on the delivery of content and interactivity to end users through multiple devices (desktop browser, cell phone, traditional phone, PDA, IM client, etc.). This new breed of application, in my humble opinion, deserves its own buzzword &#8212; &#8220;Multi-mobile&#8221; applications.

Fans of [XHTML+Voice](?page_id=13) and [SALT](?page_id=12) will know instantly what [multimodal applications](http://en.wikipedia.org/wiki/Multimodal) are &#8212; applications that support different kinds of user input (i.e., voice, keypad, stylus) through the same interface. Multi-mobile applications are differnet. These applications deliver content that is specifically tailored to the device being employed by the end user. This content is formatted differently based on the requirements of each specific device, and the mode of input is restricted to that supported by the device. Users accessing a form requiring specific input will see different manifestations of the same interface depending on the device they are using. What makes these applications special is that a single code base provides the functionality for device independence. So, whether an end user is making a phone call, using a WAP-enabled browser or an XHTML-enabled PDA the application is built to format an interface appropriately for each device.

I think this is a pretty cool concept, one worthy of its own buzzword. Besides, describing this approach to application design literally &#8212; &#8220;parallel mono-modality&#8221; &#8212; doesn&#8217;t do it justice in my opinion (needs more zing!).

**Building Multi-Mobile Applications**

One of the most convenient and powerful ways for building multi-mobile applications that I have come across is a PHP class library called [HAWHAW](http://www.hawhaw.de/) (HTML and WML Hybrid Adapted Webserver). The HAWHAW toolkit actually has two additional elements &#8212; HAWHAW XML (an XML-based markup language for creating mobile applications) and HAWXY (a proxy server written in PHP for precessing HAWHAW XML). I&#8217;ll be focusing on the HAWHAW PHP class library in this example.

The HAWHAW class library comes with [an excellent tutorial](http://www.hawhaw.de/faq.htm) and a very detailed set of [documentation](http://www.hawhaw.de/ref/php/index.html). Once I downloaded this library and began playing with it, I was able to create cross-platform multi-mobile applications in a very short period of time. To demonstrate how easy it is to use this tool, lets take an example that could apply to a number of different government entities &#8212; utility meter reading.

This issue is on my mind lately because I came home from the office recently to find that I had missed the monthly visit from the meter reader and instead had been given one of those blank forms to fill out after examining the water meter in my basement. The way my local government approaches this issue is to let customers self report their meter readings using a phone-in service (a very clunky sounding old IVR system). Lots of other municipalities use web-forms to allow customers to self report. And even though many municipalities and utilities are moving toward [Automated Meter Reading](http://www.csu.org/customer/meter/page4795.html) (where a special meter installed in customer homes emits a radio signal that is picked up by passing meter readers), customer self reporting is bound be to be a common way to get customer meter readings for a while.

**A Multi-Mobile Meter Reading Application**

This example assumes that you have downloaded the HAWHAW PHP class library and installed it on a PHP-enabled web server and have familiarized yourself with the documentation. I&#8217;d also reccomend becoming familiar with some of the general concepts of Wireless Markup Language (WML) applications development &#8212; HAWHAW was originally created to serve up WML content, so it will help if you have a basic understanding of decks and cards as they are used in the world of WML.

>   * To test the application disucssed here, point your desktop browser, micro browser, PDA, WAP-enabled cell phone, etc. [here](http://www.voiceingov.org/xfiles/hawhaw/meterReading.php).
>   * To test the VoiceXML version of this application, use your [VoIP phone](http://community.voxeo.com/fwd.jsp) to call SIP:9991421352@sip.voxeo.net. 
> 
> <img src="http://skype.hawhaw.de/blank.gif" style="border:0;" />
> 
> document.hawform.callbutton.value = &#8221; Make SkypeOut call NOW!!! &#8220;;

We begin our application by including the required HAWHAW class and setting up a deck:

<pre>// Include class to generate output
require('path_to_hawhaw/hawhaw.inc.php');

// Create page an use simulator to test and debug
$page = new HAW_deck(&ldquo;Submit Your Meter Reading&ldquo;);
$page-&gt;use_simulator();
</pre>

Calling the use_simulator() method allows users to view HAWHAW applications as they might appear in a mobile device by using a standard desktop browser. (You can still view HAWHAW applications using a traditional desktop browser without calling this method, but the application will return a simple HTML interface.)

Now lets create a form with input items for entering a meter reading, and then generate a page:

<pre>// Create a new form to enter a meter reading
$form = new HAW_form(&ldquo;meterSubmit.php&ldquo;);

// Create a submit button for the form
$submit = new HAW_submit(&ldquo;Submit&ldquo;);

$input1 = new HAW_input(&ldquo;account_num&ldquo;, &ldquo;&ldquo;, &ldquo;Account Number:&ldquo;);
$input1-&gt;set_size(6);
$input1-&gt;set_maxlength(6);
$input1-&gt;set_voice_text(&ldquo;Please enter your 6 digit account number.&ldquo;);
$input1-&gt;set_voice_type(&ldquo;digits?length=6&ldquo;);
$input1-&gt;set_br(2);

$input2 = new HAW_input(&ldquo;reading&ldquo;, &ldquo;&ldquo;, &ldquo;Meter Reading:&ldquo;);
$input2-&gt;set_size(8);
$input2-&gt;set_maxlength(8);
$input2-&gt;set_voice_text(&ldquo;Please enter your 8 digit meter reading.&ldquo;);
$input2-&gt;set_voice_type(&ldquo;digits?length=8&ldquo;);
$input2-&gt;set_br(1);

// Add input items and submit button to form
$form-&gt;add_input($input1);
$form-&gt;add_input($input2);
$form-&gt;add_submit($submit);

// Add form to page
$page-&gt;add_form($form);

// Render the page
$page-&gt;create_page();
</pre>

You can see where I have called the appropriate methods on the HAW\_input objects to style them for different types of user agents. Each of my two HAW\_input objects has a maximum character length set, and each has a line break or two that follow them. I&#8217;ve also called the set\_voice\_text() and set\_voice\_type() methods on these objects to give them the features they will need to work properly on a VoiceXML platform. From a VoiceXML perspective, you can think of these input objects as <field> elements. These methods set the field prompt and field type respectively. Note that in calling the set\_voice\_type() method, I&#8217;ve set a length for each field &#8212; if an input string of a different length is used, the VoiceXML platform will throw a nomatch event to be caught by the appropriate handler.

Speaking of event handlers, you may be wondering how to set up special prompts when noinput or nomatch events occur. HAWHAW has a [number of different methods](http://www.hawhaw.de/faq.htm#VoiceXML5) for setting up these specific event handlers, and [a lot more](http://www.hawhaw.de/faq.htm#VoiceXML0). A good deal of thought has gone into rendering VoiceXML markup through HAWHAW, and the project creator (Norbert Huffschmid) is to be commended for this excellent work. I&#8217;m going to hold off on setting up noinput and nomatch handlers for now, because I want to take a slightly different approach than what is currently supported in the HAWHAW library.

To finish out our example, we need to post the user input back to our server and take the user to another HAWHAW deck object. In a production application, this posting would probably involve validating the user input and querying an account database. For this example, we&#8217;ll keep it simple. (Note &#8212; the parameter used when creating a new HAW_form object is the URL that the user input is posted to &#8212; in this example, a file called &ldquo;meterSubmit.php&ldquo;. Lets look at that file now:

<pre>require('path_to_hawhaw/hawhaw.inc.php');
$page = new HAW_deck(&ldquo;Submission Result&ldquo;, HAW_ALIGN_CENTER);
$page-&gt;use_simulator();

// Create short variable names for form input variables
$account = trim(addslashes($_REQUEST['account_num']));
$reading = trim(addslashes($_REQUEST['reading']));


// Check for errors in user input
if (!$account || !$reading) {
$error_text1 = new HAW_text(&ldquo;You must enter both your account number and meter reading.&ldquo;);
$error_text1-&gt;set_br(2);
//$error_text1-&gt;set_voice_text(&ldquo;You must enter both your account number and meter reading.&ldquo;);
$link = new HAW_link(&ldquo;Try Again&ldquo;,&ldquo;meterReading.php&ldquo;);
$page-&gt;add_text($error_text1);
$page-&gt;add_link($link);
$page-&gt;create_page();
die;
}

if (strlen($account)!=6) {
$error_text2 = new HAW_text(&ldquo;You must enter your 6 digit account number.&ldquo;);
$error_text2-&gt;set_br(2);
//$error_text2-&gt;set_voice_text(&ldquo;You must enter your 6 digit account number.&ldquo;);
$link = new HAW_link(&ldquo;Try Again&ldquo;,&ldquo;meterReading.php&ldquo;);
$page-&gt;add_text($error_text2);
$page-&gt;add_link($link);
$page-&gt;create_page();
die;
}

if (strlen($reading)!=8) {
$error_text3 = new HAW_text(&ldquo;You must enter your 8 digit meter reading.&ldquo;);
$error_text3-&gt;set_br(2);
//$error_text3-&gt;set_voice_text(&ldquo;You must enter your 8 digit meter reading.&ldquo;);
$link = new HAW_link(&ldquo;Try Again&ldquo;,&ldquo;meterReading.php&ldquo;);
$page-&gt;add_text($error_text3);
$page-&gt;add_link($link);
$page-&gt;create_page();
die;
}

// Display results
$text1 = new HAW_text(&ldquo;Account #: $account&ldquo;);
$text1-&gt;set_br(2);
$text1-&gt;set_voice_text(&ldquo;You entered account number, $account&ldquo;);

$text2 = new HAW_text(&ldquo;Reading: $reading&ldquo;);
$text2-&gt;set_br(2);
$text2-&gt;set_voice_text(&ldquo;You entered a reading of, $reading&ldquo;);

$text3 = new HAW_text(&ldquo;Thank you!&ldquo;);
$text3-&gt;set_voice_text(&ldquo;Thank you.&ldquo;);

$page-&gt;add_text($text1);
$page-&gt;add_text($text2);
$page-&gt;add_text($text3);

$page-&gt;create_page();
</pre>

Pretty straightforward &#8212; there is probably a much more efficient way to validate the user input, but for the purposes of illustration I thought I&#8217;d focus on clarity rather than efficiency. Notice that I have commented out the text to be used on a voice platform when incomplete input is submitted. This is because it should never be needed. Remember, we parametrized our voice fields in the previous file by requiring a specific number of digits as valid input &#8212; set\_voice\_type(&ldquo;digits?length=8&ldquo;). When a field with this type is visited by the Form Interpretation Algorithm (FIA) on a VoiceXML platform, a string of digits with the appropriate length must be entered, otherwise a nomatch event will be thrown and the FIA will revisit this field until it is successfully filled (or until a nomatch event handler specifies different behavior, like transferring a caller to a customer service representative). Therefore, on a voice platform a caller should never encounter the page &ldquo;meterSubmit.php&ldquo; until both fields on the preceding page are filled with input of the appropriate length.

Clearly in production this part of the application would be much more sophisticated, but this is sufficient to illustrate the basic steps involved.

**One Step Beyond, Modifying the HAWHAW Class Library**

As powerful and as useful as this tool is, it does have its limitations. Most are inherent in the nature of developing interfaces for multiple end user devices. Because this tool needs to generate markup for a large number of user agents, many of which have character and space limitations (the HAWHAW documentation notes that &ldquo;a lot of older WAP devices can not handle more than about 1,400 bytes of compiled data.&ldquo;) it pays to keep the interface simple.

Moreover, because this tool was initially developed with the goal of generating WML pages there are some aspects of VoiceXML application development that are missing. For example, what if we wanted to set up an event handler with application scope that was specially tailored for callers having problems using the application. We may want to transfer such callers to a customer service agent so that they could get the help they need to submit their meter readings. Such an event handler might look like the following:

<pre>&lt;noinput count=&ldquo;3&ldquo;&gt;
&lt;goto next=&ldquo;#transfer&ldquo;/&gt;
&lt;/noinput&gt;

&lt;!-- Form for transferring callers that are having trouble --&gt;
&lt;form id=&ldquo;transfer&ldquo;&gt;
&lt;block&gt;
&lt;prompt&gt;Please hold a moment.  I'll transfer you to a customer service representative.&lt;/prompt&gt;
&lt;/block&gt;
&lt;transfer name=&ldquo;tran1&ldquo; bridge=&ldquo;true&ldquo; connecttimeout=&ldquo;20s&ldquo; dest=&ldquo;tel:+12223334444&ldquo;&gt;
 &lt;filled&gt;
  &lt;if cond=&ldquo;tran1 == 'busy'&ldquo;&gt;
  &lt;prompt&gt;The line is busy.  Please try back later.&lt;/prompt&gt;
  &lt;exit/&gt;
 &lt;elseif cond=&ldquo;tran1 == 'noanswer'&ldquo;/&gt;
  &lt;prompt&gt;No one is available to take your call right now.  Please try back later.&lt;/prompt&gt;
  &lt;exit/&gt;
 &lt;/if&gt;
&lt;/filled&gt;
&lt;/transfer&gt;
&lt;/form&gt;
</pre>

We might also want to capture the phone number of the caller, and set up a custom event handler to catch application errors, or to detect when a caller has become frustrated and hangs up:

<pre>&lt;!-- Capture caller ANI --&gt;
&lt;var name=&ldquo;ani&ldquo; expr=&ldquo;session.telephone.ani&ldquo;/&gt;

&lt;catch event=&ldquo;connection.disconnect.hangup&ldquo;&gt;
  &lt;var name=&ldquo;errType&ldquo; expr=&ldquo;_event&ldquo;/&gt;
   &lt;submit next=&ldquo;error.php&ldquo; namelist=&ldquo;ani errType&ldquo;/&gt;
  &lt;exit/&gt;
&lt;/catch&gt;
</pre>

The way that I do this in my VoiceXML applications is to create an [application root document](http://studio.tellme.com/vxml2/ovw/applications.html#ref_app_root), and then reference that root document in successive VoiceXML pages. There isn&#8217;t a way to do this currently in HAWHAW, but with some minor tweaks, we can add some new functionality to allow this. You can obtain a modified version of HAWHAW (along with a description of the changes) by [clicking here](http://www.voiceingov.org/xfiles/hawhaw/hawhaw_mod.zip). Note, this isn&#8217;t the official version of this tool, but one of the nice things about open source software is that you can modify it to meet your needs. Hopefully you will find this helpful &#8212; if not, the [official version](http://www.hawhaw.de/#download) of this tool can be downloaded from the HAWHAW website.

So, with our modified version, we would change the approach outlined above to set our application root document when creating a HAW_deck object.

<pre>// Include class to generate output
require('pathtohawhaw/hawhaw.inc.php');

// Create page an use simulator to test and debug
$page = new HAW_deck(&ldquo;Submit Your Meter Reading&ldquo;);
$page-&gt;use_simulator();

// Set application root for VoiceXML output
$page-&gt;set_application(&ldquo;appRoot.vxml&ldquo;);
</pre>

I put all of my application-scoped variables and event handlers in a file called &ldquo;appRoot.vxml&ldquo; and pass the name of this file as a parameter when calling the new set_application() method. Now, when HAWHAW outputs VoiceXML, there will be a reference to this root document and the variables and event handlers I have set up will be available from within my HAWHAW pages.

A quick deployment note, when setting up a VoiceXML platform to support this application, it needs to point to the &ldquo;appRoot.vxml&ldquo; file, not to the initial HAWHAW page. Users with other mobile devices will access the the HAWHAW pages directly. This may add an additional level of complexity, but since users accessing the application via phone will be calling a telephone number (not accessing a URL) its not likely to be confusing.

Although this approach is not supported in the official version of HAWHAW, I find it more conveneint for some of the very specific behavioral features I typically put in my VoiceXML applications. The fact that users can also access my application on a mobile device or PDA is icing on the cake!

**Conclusion**

You can obtain all of the code discussed above by [clicking here](http://www.voiceingov.org/xfiles/hawhaw/meter_reading_app.zip).

Hopefully, this brief introduction will get you thinking about building multi-mobile applications. I hope that it also gets you excited about the power of PHP generally and HAWHAW specifically for creating such applications. I have run the application described here on both Windows and Linux under PHP 4.3.x and 5.0 without issue.

If that can&#8217;t get you excited then perhaps there is a need for another new buzzword to describe your current state. I suggest CUDDLE (Confused, Unusually Down and Depressed and Lacking Excitement). 😉

<!-- Creative Commons License -->

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/"><img alt="Creative Commons License" border="0" src="http://creativecommons.org/images/public/somerights20.gif" /></a>
  
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/">Creative Commons Attribution-NonCommercial-ShareAlike 2.5 License</a>.

<!-- /Creative Commons License -->