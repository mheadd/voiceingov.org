---
id: 92
title: Interpreting Spoken Input
date: 2006-06-02T08:51:09+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=92
permalink: /interpreting-spoken-input/
categories:
  - General Discussion
  - Tutorials
---
Since the VoiceXML Forum Community Bulletin Board is increasingly besieged by spammers, going forward Iâ€™m going to cross post responses I submit there on this site so that interested parties (assuming there are any ;-)) can read them.

This response relates to the use of semantic interpretation in VoiceXML applications, something I have [written on before](http://www.voiceingov.org/blog/?p=82). I hope readers find the exchange below helpful.

**QUESTION:**

Is there a way to map a response to a certain value? For instance, if the user says â€œyes,â€ â€œsure,â€ or â€œyeahâ€ I&#8217;d like to put 1 in the database? If the user says â€œno,â€ â€œnope,â€ or â€œnahâ€ I&#8217;d put 0.

**ANSWER:**

There are a couple of option open to you if all you are using is a simple yes/no grammar.

Option 1 = use the builtin &#8220;boolean&#8221; grammar type. By specifying a field type of &#8220;boolean&#8221;, an implicit grammar is created that should cover affirmative or negative responses for whataver language is being used. A boolean field returns a JavaScript string based on what the user says (e.g., yes=&#8217;true&#8217; or no=&#8217;false&#8217;). You can convert this to a 1 or 0 using a simple if/else construct and a predefnied variable.

`</p>
<p><var name="convert" expr="0"/></p>
<p>...</p>
<p><field name="F_1" type="boolean"><br />
<prompt>Do you think VoiceXML rocks?</prompt><br />
<filled></p>
<p><!-- If the user says yes, then the expression in the "cond" attribute will evaluate to true --><br />
<if cond="F_1"><br />
<assign name="convert" expr="1"/><br />
</if></p>
<p><!-- If the preceding if statement did not execute, then expression in the cond attribute evaluated to false.  User said no, so we keep our original value of 0 --><br />
<submit next="mypage.jsp" namelist="convert"/></p>
<p></filled><br />
</field></p>
<p>`

Option 2 = you can use the <tag> element with a custom yes/no grammar to return a 1 or a 0. (Check your platform vendor&#8217;s documentation on this element, as there is some variation.) 

`</p>
<p><!-- In your VoiceXML document, reference the yes/no grammar --><br />
<field name="F_1"><br />
<grammar src="yesno.grxml"/><br />
...</p>
<p><!-- Contents of yesno.grxml file --><br />
< ?xml version = "1.0"?><br />
<grammar xml:lang="en-US" version="1.0" root="R_1" type="application/srgs+xml" xmlns="http://www.w3.org/2001/06/grammar"><br />
<rule id="R_1"><br />
<one -of><br />
  <item>yes <tag>F_1=1;</tag> </item><br />
  <item>yeah <tag>F_1=1;</tag> </item><br />
  <item>hells yeah <tag>F_1=1;</tag> </item><br />
  <item>yur damn skippy <tag>F_1=1;</tag> </item><br />
  <item>no <tag>F_1=0;</tag> </item><br />
  <item>nope <tag>F_1=0;</tag> </item><br />
  <item>no way <tag>F_1=0';</tag> </item><br />
  <item>hells no <tag>F_1=0;</tag> </item><br />
</one><br />
</rule><br />
</grammar></p>
<p></field</p>
<p>`

This has the effect of filling the field named &#8220;F_1&#8221; with the value specified in the <tag> when one of the grammar items is recognized. A few good links to get you started follow:

<a href="http://cafe.bevocal.com/docs/vxml/tag.html#288994" target="_blank">BeVocal Cafe</a>

<a href="http://docs.voxeo.com/voicexml/2.0/tag.htm" target="_blank">Voxeo Community</a>