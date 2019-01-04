---
id: 19
title: RSS To VXML Pt. II
date: 2005-06-19T16:33:47+00:00
author: mheadd
layout: page
guid: http://www.voiceingov.org/blog/?page_id=19
original_post_id:
  - "19"
---
**Building a &#8220;Smarter&#8221; Voice Feed**

One of the many nice things about RSS feeds is that they are updated regularly &ndash; this means that your voice application will provide a steady stream of updated headlines or events. Unfortunately, it means that you have less control over how the content is presented. Its always a good practice in developing VoiceXML applications to prerecord prompt and messages &ndash; this helps avoid user confusion when a text-to-speech engine has difficulty pronouncing a word. But because RSS feeds are so dynamic, prerecording prompts is not possible.

This wouldn&#8217;t be such a problem if RSS didn&#8217;t have its roots in news delivery. Because RSS feeds are typically used to provide up to the minute headlines or news stories, the content of these feeds may not lend themselves to aural presentation. Headlines are meant to be eye-catching, not ear-catching. This is particularly true of technology feeds, which can be littered with
  
acronyms, abbreviations and other strange anomalies.

For example, consider the following example headline: &ldquo;IBM introduces new UNIX OS to support customer IT efforts.&rdquo; Depending on which TTS engine is being used, your results on rendering this written sentence as spoken text will vary. Some TTS engines will assume that words composed of all upper case letters are meant to be sounded out by individual letters (for example, IBM would be rendered in speech as eye-bee-em). However, the above sentence shows the limitations of such an approach. The word UNIX is typically written in upper case but is pronounced &ldquo;yoo-niks.&rdquo; Fortunately, with XSLT it is possible to create &#8220;smarter&#8221; voice feeds.

**Replacing Words**

Using XSLT templates, it is possible to identify troublesome words or text strings and replace them with other, more easily pronounceable words or phrases. These replacements can be the full spelling of abbreviated words, or the phonetic spelling of acronyms and other pesky items. I&#8217;ll skip the details in this tutorial because there is an excellent overview of this technique available on [XML.com](http://www.xml.com/pub/a/2002/06/05/transforming.html). My example of a &#8220;search and replace&#8221; XSLT document to improve the pronunciation quality of the of the CNET news RSS feed we have been working with [can be found here](../xfiles/tutorials/cnet_news_vxml2.xsl).

Once you have your search and replace XSLT file, you have some decisions to make. You must decide what words you want to replace as your XSLT file converts the source RSS data into VoiceXML. You must also decide what you want to replace these words with. For acronyms or abbreviated words, I recommend replacement using the entire word (for example, replace OS with Operating System). This will probably be the cleanest and most efficient way of ensuring your users hear the headlines correctly.

**Alternate Approaches**

VoiceXML provides a way to phonetically pronounce words. The [<phoneme>](http://www.w3.org/TR/speech-synthesis/#S3.1.9) element provides guidance to the TTS engine to instruct it on how to pronounce words properly. Note &ndash; this element is actually part of the Speech Synthesis Markup Language (SSML) specification, as is the <voice> element discussed previously, but it is available for use in VoiceXML. (There is some excellent information on this available from the [VoiceGenie](http://developer.voicegenie.com/voicexml2tagref.php?tag=ssvs_phoneme&display=ssmltags) web site.) 

The SSML specification also provides another option for dealing with words that are sometimes troublesome for TTS engines to pronounce. The [<say-as>](http://www.w3.org/TR/speech-synthesis/#edef_say-as) element allows developers to specify the kind of data to be rendered as speech, to help the TTS engine pronounce it properly. This element has two different attributes (&#8220;format&#8221; and &#8220;interpret-as&#8221;) to provide the appropriate context for the text to be rendered as speech to the
  
caller.

For example, to help a TTS engine properly render an e-mail address, the following code could be used:

<pre>&lt;prompt&gt; This is a test to show proper rendering of an email address
&lt;break size=&ldquo;100&rdquo;/&gt;
&lt;say-as interpret-as=&ldquo;net&rdquo; format=&ldquo;email&rdquo;&gt;tester@mydomain.com&lt;/say-as&gt;<br />
&lt;/prompt&gt;
</pre>

This is nice, but the options provided for the <say-as> element are somewhat limited. As a result, the implementation of this specification can very greatly between different VoiceXML hosting providers. If you decide to take the <say-as> approach to deal with unique pronunciations, you should read the implementation materials for your provider carefully. Information from the major VoiceXML hosting providers can be reviewed at the following links:

  * [BeVocal](http://cafe.bevocal.com/docs/vxml/say-as.html#287683) Hosting Platform (custom implementation)
  * [TellMe](http://studio.tellme.com/vxml2/ref/ssml/say-as.html) Hosting Platform (custom
  
    implementation)
  * [Voxeo](http://community.voxeo.com/vxml/docs/vxml_m_2.0/say-as.htm) Hosting Platform (appears to conform to SSML specification)

**Conclusion**

This tutorial provides a basic overview of how to convert RSS feeds into dynamic VoiceXML. It demonstrates the power of XSLT to convert content in one XML format to VoiceXML. XSLT is a powerful enabler of VoiceXML documents and applications, as subsequent tutorials will demonstrate.

<!-- Creative Commons License -->

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/"><img alt="Creative Commons License" border="0" src="http://creativecommons.org/images/public/somerights20.gif" /></a>
  
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/">Creative Commons Attribution-NonCommercial-ShareAlike 2.5 License</a>.

<!-- /Creative Commons License -->