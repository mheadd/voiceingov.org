---
id: 136
title: Accessing Web Services From CCXML
date: 2008-04-21T20:40:05+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=136
permalink: /accessing-web-services-from-ccxml/
original_post_id:
  - "136"
categories:
  - Tutorials
---
This is the first in a series of posts that will highlight how to accomplish specific things using the [Voxeo Prophecy](http://voxeo.com/prophecy/) platform. All of the examples that will be discussed draw directly from the [GreenPhone project](http://www.voiceingov.org/blog/?p=135) discussed in a previous post.

The first issue that will be discussed â€“ accessing web services from CCXML using PHP.

One of the very cool things about Prophecy is that it comes bundled with the PHP scripting language. In fact, I have on occasion referred to PHP as Prophecy&#8217;s &#8220;embedded scripting language.&#8221; PHP 5 comes with an abundance of features that will be of interest to IVR developers â€“ chief among them, the ability to create [SOAP clients](http://us2.php.net/manual/en/ref.soap.php) to interact with web services, and the ability to easily work with data in XML format using the [SimpleXML](http://us2.php.net/manual/en/book.simplexml.php) extension.

If you&#8217;ve read my previous post on the GreenPhone Project, you will know that I am using a collection of web services from [StrikeIron](http://strikeiron.com/) that includes a web service to provide information on U.S. area codes. If we pass this web service an area code, it will return the U.S. state that area code is in. Ultimately, what the GreenPhone application will do is look up E85 and bio-diesel stations by state. So when a person calls the application, we want to use their area code to look up what state they are in â€“ thereby saving them the trouble of entering this information manually.

In CCXML, we can access the caller&#8217;s ANI via the Connection Object:
  
`<br />
<transition state="initial" event="connection.alerting"><br />
&nbsp;<log expr="'*** Call is coming in.  Lookup area code information. ***'"/><br />
&nbsp;<assign name="ani" expr="event$.connection.remote"/><br />
&nbsp;<assign name="areacode" expr="ani.substring(0,3)"/><br />
&nbsp;<send target="'php/areaCodeLookup.php'" name="'lookupEvent'" targettype="'basichttp'" namelist="areacode"/><br />
</transition><br />
` 
  
This block of code show how to set up a transition to access the ANI on an incoming call. When an incoming call is detected by Prophecy, the &#8220;connection.alerting&#8221; event is delivered and we have access to the Connection Object&#8217;s &#8220;remote&#8221; property â€“ this property exposes the telephone URL for the device that is calling into the platform. Note â€“ in [my previous post](http://www.voiceingov.org/blog/?p=135), I explained the process of setting up the Prophecy SIP phone to deliver a specific ANI. This is how we access the value that is set in the Prophcy SIP phone.

We assign the ANI value to a variable we have previously declared and (very cleverly) called &#8220;ani&#8221;, and then we grab the first 3 characters of this string (using the ECMAScript substring method) and assign them to another variable called &#8220;areacode&#8221;. We then pass the area code value to a PHP script that will interact with the StrikeIron area code web service.

Using the [CCXML <send/> element](http://www.w3.org/TR/2007/WD-ccxml-20070119/#Send) in this fashion is identical to an HTTP GET with the areacode variable appended to the URL of the PHP script, like this:
  
`<br />
http://myserver/php/areaCodeLookup.php?areacode=123<br />
` 

There several possible outcomes of this HTTP request:

  1. Our PHP script was able to successfully interact with the StrikeIron web service and lookup the U.S. state information for the submitted area code; 
  2. Our PHP script was able to successfully interact with the StrikeIron web service but was not able to lookup the state information for the submitted area code (bad area code);
  3. Something went wrong (an exception occurred) while trying to interact with the web service; or,
  4. Something really went wrong and our HTTP request resulted in a bad response from the server.

We need to set up handlers for each possible outcome â€“ we won&#8217;t discuss them in detail until after we look more closely at the PHP components that are interacting with the StrikeIron web service, but to summarize what we&#8217;ll need, here they are:

<pre>&lt;transition state="lookup" event="areaCodeLookupSuccess"&gt;
&lt;/transition&gt;

&lt;transition state="lookup" event="areaCodeLookupFailure"&gt;
&lt;/transition&gt;

&lt;transition state="lookup" event="error.send.failed"&gt;
&lt;/transition&gt;
</pre>

The first two handlers react to custom events that we will toss into the CCXML event stream (more on that shortly), and the last will take care of instances where we get an invalid response back from the server (e.g., a 404 response). Now lets look at the PHP components that interact with the StrikeIron web services.

When the HTTP request from Prophecy that holds our area code information is received in PHP, we can access the submitted value by using the PHP [$_REQUEST superglobal](http://us2.php.net/manual/en/reserved.variables.request.php):
  
`<br />
$areacode = (int) $_REQUEST['areacode'];<br />
` 

You&#8217;ll notice that we also [typecast](http://us3.php.net/manual/en/language.types.type-juggling.php#language.types.typecasting) the value as a way of cleansing the input â€“ as with any other kind of web application, never trust user input. Even though we&#8217;re not using the submitted information in a SQL query, this is a really good habit to get into. There are certainly other ways to achieve this, but type casting is simple and effective for our purposes.

The PHP version that comes bundled with Prophecy has support for PHP&#8217;s SOAP extension right out of the box. Since we&#8217;re going to be accessing several different web services over the course of one telephone call, I decided to set up a very simple class to handle all of the interactions with the StrikeIron web services.

<pre>class greenSoapClient {

  private $client;
  private $headers;

  function __construct($type) {
    global $WSDL, $USER, $PSWD;
    $this-&gt;client = new SoapClient($WSDL[$type], array('trace' =&gt; 1,
                                          'exceptions' =&gt; 0));
    $headerArray = array("RegisteredUser" =&gt; array("UserID" =&gt; $USER,
                                          "Password" =&gt; $PSWD));
    $this-&gt;headers = new SoapHeader("http://ws.strikeiron.com",
                                          "LicenseInfo", $headerArray);
  }

  function makeSoapCall($name, $params) {
    $result = $this-&gt;client-&gt;__call($name, array($params), NULL, $this-&gt;headers);
    return $this-&gt;client-&gt;__getLastResponse();
  }

  function __destruct() {
    unset($this-&gt;client);
  }

}
</pre>

This class has only three functions â€“ a constructor, a destructor and a function to make the call to the SOAP method we want information from.

When we instantiate the greenSoapClient class, we pass in a reference to a WSDL file for the service we want to invoke. In this case, we will pass in a reference to the WSDL file for the U.S. Area Code Information Web Service. (Actually, the string &#8220;areaCode&#8221; is used to access the WSDL reference from a pre-established associative array holding the URL references for all of the WSDL files used by the greenPhone application.)
  
`<br />
$mySoapClient = new greenSoapClient("areacode");<br />
` 

Now that we have our area code information, and a shiny new greenSoapClient object to work with, we can make our SOAP call:
  
`<br />
$param = array('AreaCode' => $areacode);<br />
$response = $mySoapClient->makeSoapCall('GetAreaCode', $param);<br />
` 

The variable $response now holds the XML response that was returned from the web service. We&#8217;ll need to process this response in order to properly format the information we want to return to CCXML.

One of the very cool things about the [Voxeo implementation of CCXML](http://docs.voxeo.com/ccxml/1.0-final/appendixg_ccxml10.htm) is that developers can toss custom events into the CCXML event stream using simple HTTP responses. Prophecy lets us send back a custom event, as well as any data that we want to access in CCXML as properties of that event. We do this by formatting our response as follows:

First line of body of HTTP response = custom event name.
  
Data to be returned to CCXML = name value pair appearing on successive lines of the HTTP body, one pair per line.

The U.S. Area Code Information Web Service returns two pieces of information that we want to access in CCXML â€“ a count of the number of locations identified for each area code (typically 1), and the name of the U.S. state that area code belongs to. A snippet of the raw response returned from the web service might look something like this (for the 610 area code):
  
`<br />
<ServiceResult><br />
&nbsp;<Count>1</Count><br />
&nbsp;<AreaCodes><br />
&nbsp;&nbsp;<AreaCodeInfo><br />
&nbsp;&nbsp;&nbsp;<AreaCode>610</AreaCode><br />
&nbsp;&nbsp;&nbsp;<Location>Pennsylvania</Location><br />
&nbsp;&nbsp;</AreaCodeInfo><br />
&nbsp;</AreaCodes><br />
</ServiceResult><br />
` 

We want to format our raw XML response like so:
  
`<br />
areaCodeLookupSuccess<br />
count=1<br />
location=Pennsylvania<br />
` 

The easiest way to do this in PHP is to use the SimpleXML extension:
  
`<br />
$xml = new SimpleXmlElement($response);<br />
$result = $xml->soapBody->GetAreaCodeResponse->GetAreaCodeResult;<br />
$output = "areaCodeLookupSuccessn";<br />
$output .= "count=".$result->ServiceResult->Count."n";<br />
$output .= "location=".$result->ServiceResult->AreaCodes->AreaCodeInfo->Location."nn";<br />
` 

We take the response from the StrikeIron web service and use it to create a new SimpleXML object. We can then access the values we want and build our HTTP response.

How do we deliver our response once we&#8217;re done constructing it, we simply use the PHP &#8220;echo&#8221; language construct to write it out:
  
`<br />
echo $output;<br />
` 

Now that we&#8217;ve returned our values to CCXML, how do we access them? For the answer to that,we need to go back to the handlers we set up previously, most importantly the handler for the custom &#8220;areaCodeLookupSuccess&#8221; event:
  
`<br />
<transition state="lookup" event="areaCodeLookupSuccess"><br />
&nbsp;<assign name="count" expr="event$.count"/><br />
&nbsp;<if cond="count == 1"><br />
&nbsp;&nbsp;<assign name="location" expr="event$.location"/><br />
&nbsp;&nbsp;<assign name="stateCode" expr="getStateCode(event$.location)"/><br />
&nbsp;&nbsp;<assign name="myState" expr="'accepting'"/><br />
&nbsp;&nbsp;<accept connectionid="connection_id"/><br />
&nbsp;<else/><br />
&nbsp;&nbsp;<log expr="'*** Could not look up area code. ***'"/><br />
&nbsp;&nbsp;<reject/><br />
&nbsp;</if><br />
</transition><br />
` 

When we write out our web service response in PHP, we can cause a custom event to drop into the CCXML event stream â€“ the name of this event is the first line of the HTTP response we just constructed â€“ areaCodeLookupSuccess.

We access the values we just returned to CCXML as properties of the areaCodeLookupSuccess event using the &#8220;event$.&#8221; vernacular. This allows us to assign these values to ECMAScript variables that we have previously declared. It also lets us decide how we want our application to react, based on certain conditions (e.g., if count = 0).

Similarly, our other event handlers can be used if we get an unexpected response form the web service â€“ we could send back a &#8220;areaCodeLookupFailure&#8221; event. If something really bad happens â€“ like an invalid response from the web server we will get an &#8220;error.send.failed&#8221; event, so we&#8217;ll want to have a handler ready for that as well.

Now that you have a flavor for how to access web services using CCXML and PHP, we&#8217;ll look at two different techniques for returning information from a web service to VoiceXML. We&#8217;ll cover these two techniques in the next two posts. Stay tuned&#8230;