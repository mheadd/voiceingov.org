---
id: 53
title: X + V + PHP
date: 2005-08-30T10:14:16+00:00
author: mheadd
layout: page
guid: http://www.voiceingov.org/blog/?page_id=53
original_post_id:
  - "53"
---
**Introduction**

Any great chef will tell you that the perfect dish is made up of different ingredients that complement each other perfectly. And if your thing is cooking up web applications, there aren&#8217;t many sweeter ingredients that XHTML and VoiceXML.

[XHTML+Voice](http://www.voicexml.org/specs/multimodal/x+v/12/index.html) (or X+V for short) is a specification for multimodal applications that is now making its way through the W3C approval process. It was created under the auspices of the VoiceXML Forum, the same great folks that brought you VoiceXML and who continue to develop this standard.

For those that aren&#8217;t in the know, there is a seriously cool new feature in the recently released version of the [Opera Browser](http://my.opera.com/mheadd/affiliate/) (Version 8.0). Opera has added support for X+V applications. This means that developers who are comfortable with both XHTML and VoiceXML can cook up some powerful applications that let users enter input through multiple channels using the same interface, or both hear and see content rendered by the browser.

What I really like about all this is that it starts to deliver on the promise made by [advocates of proper markup](http://www.webstandards.org/) in web pages and [XHTML evangelists](http://www.alistapart.com/stories/betterliving/). These groups have been lauding the benefits of XHTML for a long time now. Unfortunately, although I agree very strongly with their message, its not clear that everyone is listening. Web developers are very practical people "“ show them how adhering to a standard will make their lives easier (or more fun) and their likely to do it.

That&#8217;s what I hope to do here. This brief tutorial will demonstrate the power of XHTML when combined with VoiceXML in a way that I hope is accessible to a large audience of developers.

**Cooking up XHTML+Voice**

The real promise of X+V (in MHO) is the potential to add voice functionality to existing visual XHTML markup. This would have enormous benefits for all kinds of users, most notably those with visual disabilities. The world is full of good, clean, well structured XHTML just waiting to be paired up with VoiceXML. Even though we don&#8217;t control this content (and, therefore, can&#8217;t do browser sniffing and send X+V markup from the server to clients to can render it), we can still combine it with VoiceXML

Since XHTML and VoiceXML are both XML vocabularies, purists might point to the [XSLT standard](http://www.w3.org/Style/XSL/) as the proper way to create X+V markup. They&#8217;d have a valid point "“ anyone who is comfortable writing stylesheet transformation should have no problem creating some XSLT to add VoiceXML to XHTML. I like this approach and I describe something similar in an [earlier tutorial](http://www.voiceingov.org/blog/?page_id=8).

But this tutorial is really geared for those that aren&#8217;t quite convinced of the real power of XHTML yet "“ for these folks, talking about the elegance of XSLT (which, like XHTML, is another XML vocabulary) probably won&#8217;t seal the deal. Using XSLT can often require the use of an additional component "“ an [XSLT engine](http://www.google.com/search?hl=en&q=xslt+engine&btnG=Google+Search) "“ that may present additional challenges for developers.

Instead, this approach will use plain old PHP to process XHTML and to add VoiceXML markup to an existing web page with no additional modules or extensions required. For those that don&#8217;t already know, PHP has some [very powerful XML processing capabilities](http://phpxmlclasses.sourceforge.net/). The new version of PHP has an extension called [SimpleXML](http://us2.php.net/simplexml) which I have found very useful in taking in XML and rendering pieces of it in other markup languages (i.e., grabbing an RSS feed and displaying news stories on a web page.) However, I haven&#8217;t found it particularly useful for creating new XML documents.

[MiniXML](http://minixml.psychogenic.com/index.html) is an open source PHP class library that can be used to parse and generate XML. The latest version can be download [here](http://minixml.psychogenic.com/download.html). (Note, there is also a PERL version available for lovers of the other "P.") This tool contains almost everything that is need to quickly and easily convert XHTML content to X+V.

**Getting Prepped**

The approach described here assumes some familiarity with using PHP classes "“ you may want to brush up on this if you don&#8217;t feel comfortable making changes to PHP class libraries or using [object-oriented PHP](http://www.webreference.com/programming/phpanth2/).

You may also want to brush up on the basics of X+V "“ there are some great tutorials listed [here](http://www.voiceingov.org/blog/?page_id=13), and also on the [Opera website](http://my.opera.com/community/dev/voice/). With that out of the way, we&#8217;re ready for the next step.

**Making a Few Changes**

Once you&#8217;ve downloaded and extracted the latest version of MiniXML (version 1.3.0 as of this writing), go to the directory you extracted it to and take a look at the file structure.

The primary file used with MiniXML is called "minixml.inc.php" &#8212; include this file at the beginning of all scripts using MiniXML. According to the documentation on the site:

<pre>All your interactions with MiniXML begin with a MiniXMLDoc object.
To create one, simply:

$xmlDoc = new MiniXMLDoc();
</pre>

Now that we have an object, we can use it to parse (read in) existing
  
XML and/or to create new XML.

Before we do this, however, we&#8217;re going to make a few modifications to some of the MiniXML files. First, open the file &#8220;minixml.inc.php" &#8211; this file contains a number of configuration settings that you can modify to change the behavior of the class. At or around line 59, you will see a constant named "MINIXML\_AUTOESCAPE\_ENTITIES" defined with a value of 1. This constant controls how special characters and symbols are handled "“ if this constant is set to a value of 1, these character are escaped (replaced with equivalent HTML entities); if its set to 0 they are not escaped (and usually left out of the generated XML). We want to change this parameter to 0 "“ this should filter out the DOCTYPE declaration found in most XHTML documents (immediately after the XML declaration). X+V requires a special DOCTYPE declaration that we&#8217;ll add in a minute.

Next, go into the folder named "classes" and open the file called "doc.inc.php." (Note "“ unlike the previous change, these changes will modify the MiniXML classes in ways not envisioned by the creators. If it makes you more comfortable, create a backup copy before making these changes.) We need to make three sets of changes to this file.

First, at or around lines 77-80 you will see the properties for the MiniXMLDoc class declared. After the last property, add a new property called "$voice" as follows:

<pre>var $voice = 'false';
</pre>

If you look below the section where the properties are declared, you will see the class methods, beginning with the constructor method. Somewhere in this section, add a new method called "setVoice()" as follows:

<pre>function setVoice() {
$this-&gt;voice = 'true';
}
</pre>

This is the method we will use to change the value of the $voice property we just added. Now that we have a new property, and a method to change that property, we need to change the behavior of the MiniXMLDoc class based on the value we give this property.

At or around line 745, you will see a method called "toString" &#8211; this is the method called when you want to generate a new XML document. We&#8217;re going to modify this method as follows:

<pre>function toString ($depth=0)
{
$retString = $this-&gt;xxmlDoc-&gt;toString($depth);

if ($depth == MINIXML_NOWHITESPACES)
{
$xmlhead = "";
} else {
$xmlhead = "n ";
}
<strong>if ($this-&gt;voice == 'true') {
$xmlhead .="n ";
}</strong>

$search = array("/]*)&gt;s*/smi", "//smi");
$replace = array($xmlhead, "");

$retString = preg_replace($search, $replace, $retString);

if (MINIXML_DEBUG &gt; 0)
{
_MiniXMLLog("MiniXML::toString() Returning XML:n$retStringnn");
}
</pre>

Very simply, we&#8217;re telling MiniXML to check the value of our $voice property "“ if it is set to &#8216;true&#8217; then we want to generate the required XHTML+Voice DOCTYPE declaration as part of our generated XML document. After these changes are made, you should still be able to use MiniXML as before "“ the only difference will be that you can now generate an X+V document. We&#8217;ll do that next "“ before proceeding, save the file you just modified.

**Turing XHTML into X+V**

Once we&#8217;ve modified the MiniXML classes as described above we&#8217;re ready to create some X+V. The first step is to identify an XHTML document that you want to convert "“ this could be a file on your hard drive, or one that is available on the web. Your choice. For this example, I&#8217;ll use one that is in one of the directories on my hard drive, and probably on your&#8217;s as well. To keep things simple, we&#8217;ll use the default index page that&#8217;s installed with the Apache web server "“ on my machine, this file is called "index.html.en" and its located in the root Apache directory.

Our goal is to take this file, which is just plain XHTML 1.0 transitional, and turn it into X+V so that we can render some of its content audibly. To do this, create a new PHP file. Now we need to make sure that we set the appropriate MIME type for X+V content "“ this will tell our browser (in this case, Opera 8.0) that it should render the content as X+V:

<pre>// Set MIME Type for X+V
header('Content-type: application/xhtml-voice+xml');
</pre>

Now we need to include the MiniXML classes and create a new object:

<pre>// Require MiniXML library and instantiate new MiniXML Object
require_once('minixml.inc.php');
$xmlDoc = new MiniXMLDoc();
</pre>

Next, we&#8217;re going to set the $voice property that we added above to tell MiniXML to generate the X+V DOCTYPE declaration when we generate our finished product.

<pre>// Set the DTD for XHTML+Voice
$xmlDoc-&gt;setVoice();
</pre>

Now we want to obtain an XML document to modify and access its root element:

<pre>// Import XHTML file and get root element
$xmlDoc-&gt;fromFile('../index.html.en');
$xmlRoot =& $xmlDoc-&gt;getRoot();
</pre>

Now we&#8217;re ready to starting adding our VoiceXML dialogs to the document. First, we access the <html> element and add a new XML namespace for XML events. (X+V is closely tied to the XML Events specification "“ if your not comfortable with this spec, [check it out here](http://www.w3.org/TR/xml-events/).) :

<pre>// Access &lt;html&gt; element and add XML events namespace declaration
$html =& $xmlRoot-&gt;getElementByPath('html');
$html-&gt;attribute('xmlns:ev', 'http://www.w3.org/2001/xml-events');
</pre>

Almost all of our new content is added within the <head> element of the XHTML page. We&#8217;ll add two simple VoiceXML dialogs "“ one will render a simple message when the page is loaded, the other will render the content of a paragraph (enclosed within <p> tags) when clicked. We haven&#8217;t added the event handlers for these dialogs yet, but we will shortly.

<pre>// Access &lt;head&gt; element and create child elements for VoiceXML forms
$head =& $xmlRoot-&gt;getElementByPath('html/head');
$childElement1 =& $head-&gt;createChild('form');
$childElement1-&gt;attribute('xmlns', 'http://www.w3.org/2001/vxml');
$childElement1-&gt;attribute('id', 'sayHello');
$newChild1 =& $childElement1-&gt;createChild('block');
$newChild1-&gt;text('Hello. Voice is enabled.');

$childElement2 =& $head-&gt;createChild('form');
$childElement2-&gt;attribute('xmlns', 'http://www.w3.org/2001/vxml');
$childElement2-&gt;attribute('id', 'talkNow');
$newChild2 =& $childElement2-&gt;createChild('block');
$newChild3 =& $newChild2-&gt;createChild('prompt');
$newChild3-&gt;attribute('xv:src', '#talk');
</pre>

Now that we&#8217;ve added our dialogs, we need to add event handlers so that our browser will know when to execute them. Note that in the dialogs above, we added an "id" attribute. The event handlers we will now add will reference these id&#8217;s and fire when the designated event occurs.

<pre>// Access &lt;body&gt; element and add VXML event handler
$body =& $xmlRoot-&gt;getElementByPath('html/body');
$body-&gt;attribute('ev:event', 'load');
$body-&gt;attribute('ev:handler', '#sayHello');

// Access first &lt;p&gt; element and associate with VXML event handler
$para =&$xmlRoot-&gt;getElementByPath('html/body/p');
$para-&gt;attribute('id', 'talk');
$para-&gt;attribute('ev:event', 'click');
$para-&gt;attribute('ev:handler', '#talkNow');
</pre>

Now that we&#8217;ve made all of the required changes, we need to output the XML so that our browser can render it:

<pre>// Print our new XHTML+Voice document
echo $xmlDoc-&gt;toString();
</pre>

Give this file a name and save it. Now when you open this file up with your Opera browser, you should hear the message: "Hello. Voice is enabled." Try clicking on the first paragraph in the page "“ the browser should read the content to you. To download a copy of this file, [click here](http://www.voiceingov.org/xfiles/tutorials/makeXV.php.txt).

You can modify the voice setting in Opera by going to Tools > Preferences and clicking on the Advanced tab. The voice options is on the lower left "“ personally I find the female voice a lot more soothing. 😉

**Conclusion**

Now that you know how to turn plain old XHTML into voice-enabled X+V using a dash of PHP and your trusty Opera browser, I bet your thinking of all sorts of new projects. This highlights the importance of proper markup and the use of XHTML. If can be so bold as to quote the right honorable Sir Tim Berners-Lee:

> "You affect the world by what you browse."

I would submit that you affect the world by **how** you browse, or by how you enable others to browse. Voice browsing is the future, and those that have taken steps to make sure their web pages use good clean XHTML are a few steps away from affecting how people can browse web content. Just a few steps away from affecting the world.