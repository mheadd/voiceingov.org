---
id: 22
title: Obfuscating User Input
date: 2005-06-19T17:09:14+00:00
author: mheadd
layout: page
guid: http://www.voiceingov.org/blog/?page_id=22
original_post_id:
  - "22"
---
**Controlling sensitive information on hosting platforms**

When deploying voice applications, the first question developers often face is whether to host their own voice gateway or outsource this function to an application service provider (ASP). To help with this question, there is a great [article in Developer.com](http://www.developer.com/voice/article.php/1569431). If you are outsourcing this function, then there are several things you will need to keep in mind &#8211; most of these are discussed in the Developer.com article. One very important consideration for governments deploying voice applications using the ASP model is the protection of citizen information.

VoiceXML ASPs like [BeVocal](http://cafe.bevocal.com/docs/tools/logbrowser.html#192640) and [TellMe](http://studio.tellme.com/help/debuglegend.html) provide suites of online tools for developers, among them are tools to help developers analyze calls. Typically, these log analyzing tools capture information that is spoken or input via DTMF by users &#8212; there are a number of instances where having this information is useful in debugging an application and/or analyzing how it is working. However, if you are creating voice applications that will be used by citizens and taxpayers, chances are these applications will utilize citizen-specific identifiers (e.g., Social Security numbers).

For some very obvious reasons, it is generally not a very good idea to have this information stored in the log files of a VoiceXML ASP. Fortunately, there is something developers can do.

**BeVocal Platform**

The BeVocal Platform provides an extension to the VoiceXML 2.0 specification to allow developers to control the logging of user input. The [bevocal.logging](http://cafe.bevocal.com/docs/vxml/index.html?content=properties.html#285537) property lets a developer manipulate when information is recorded in the trace logs used in BeVocal&#8217;s log browser tool. The following code sets the value of the bevocal.logging property to &ldquo;false&rdquo; and prevents user input from being logged (the default value is &ldquo;true&rdquo;).

<pre>&lt;property name=&ldquo;bevocal.logging&rdquo; value=&ldquo;false&rdquo;/&gt;
</pre>

The BeVocal platform also allows devleopers to have their log files (and any captured utterances) encrypted. This is accomplised through the use of naother extension &#8211; the [bevocal.securelogging.enabled](http://cafe.bevocal.com/docs/vxml/properties.html#312498) property.

**TellMe Platform**

Just like the BeVocal Platform, the TellMe platform has its own extension to control logging. TellMe provides the &ldquo;tellme.confidential&rdquo; attribute to the [Property Tag](http://studio.tellme.com/vxml2/ref/elements/property.html). One point of contrast between the two platforms &#8212; unlike the BeVocal extension, the &ldquo;tellme.confidential&rdquo; attribute must be set to &ldquo;true&rdquo; in order to control logging.

<!-- Creative Commons License -->

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/"><img alt="Creative Commons License" border="0" src="http://creativecommons.org/images/public/somerights20.gif" /></a>
  
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/">Creative Commons Attribution-NonCommercial-ShareAlike 2.5 License</a>.

<!-- /Creative Commons License -->