---
id: 33
title: Confidence-Based Recognition in VoiceXML
date: 2005-07-03T12:19:57+00:00
author: mheadd
layout: page
guid: http://www.voiceingov.org/blog/?page_id=33
original_post_id:
  - "33"
---
**Introduction**

In the February 2005 issue of VoiceXML Review (the &#8220;Speak & Listen&#8221; column), Matt Oshry of TellMe networks provides an [excellent overview](http://www.voicexmlreview.org/feb2005/columns/Feb2005_speak_listen.html) of a technique referred to as &#8220;confidence-based confirmation,&#8221; or &#8220;confidence-based recognition.&#8221; This approach is highly valuable in applications that have unique or sophisticated grammars, or where high confidence levels during recognition is absolutely essential (i.e., telephone-based voting systems).

The approach discussed here takes a slightly different approach to this technique by structuring the dialog used to confirm user input as a [subdialog](http://www.w3.org/TR/voicexml20/#dml1.3.1), making it (in my opinion) a bit easier to reuse in different parts of an application. I&#8217;m also going to demonstrate the power of a new bit of functionality contained in the developing VoiceXML 2.1 specification that allows developers to structure applications to capture the audio of a caller utterance while a recognition is performed. If you&#8217;ve read [my paper on telephone-based voting systems](http://vote.nist.gov/ecposstatements/phone_voting_whitepaper.doc), you&#8217;ll understand why I believe that this functionality is central to VoiceXML-enabled telephone voting.

By way of a little background, there are other ways to raise the required level of confidence for recognitions in VoiceXML applications. For example, [the &#8220;confidencelevel&#8221; property](http://www.w3.org/TR/voicexml20/#dml6.3.2) allows developers to specify the degree of accuracy needed for recognitions in a VoiceXML application. Changing the value of this property from its default setting of .5 (50 percent confidence) to .80 will require a higher degree of accuracy before a successful recognition will occur

<pre>&lt;property name=&ldquo;confidencelevel&ldquo; value=&ldquo;.8&ldquo;/&gt;
</pre>

However, this is a rather blunt instrument to use if you require a higher degree of accuracy for very specific recognitions. Changing the value of this property will raise the required confidence level for all recognitions within the scope where this property is changed. If this property is set within the application root document, it will have application level scope and apply to all recognitions in the application (probably not a good thing). Additionally, simply changing the value of this property will do nothing to change the way a platform handles a &#8220;nomatch&#8221; event. You&#8217;ll still need to craft some custom event handlers for this, unless you want some frustrated callers.

**Platform Support for VoiceXML 2.1**

While support for the VoiceXML 2.1 specification is growing, I could only identify one major platform that supports the capturing of caller utterances during a recognition &#8212; [BeVocal](http://cafe.bevocal.com). Section 7 of the [VoiceXML 2.1 specification](http://www.w3.org/TR/2005/CR-voicexml21-20050613/) states that in order to enable recording during recognition, the value of the &#8220;recordutterance&#8221; property must be set to &#8216;true&#8217;. Several other platforms have something close (and may have changes in the pipeline to support this part of the 2.1 specification), but they won&#8217;t fit our needs here:

  * TellMe &#8211; the [Tellme platform](http://studio.tellme.com) has a proprietary extension to the <property> element called &#8220;tellme.field.recordutterance&#8221;. This extension does allow for the capturing of utterances when a recognition is performed. However, the TellMe document states that &#8220;The utterance is recorded regardless of whether the recognition succeeded or resulted in a nomatch.&#8221; While this is great if you are trying to fine tune a grammar, it won&#8217;t do for our example here.
  * Voxeo &#8211; the [Voxeo platform](http://community.voxeo.com) has an extension for a proprietary element called &#8220;<voxeo:recordcall>&#8221; that allows developers to designate a specific percentage of calls to be recorded for quality purposes. According to the Voxeo documentation, this element &#8220;allows the developer to record both sides of a call, recording the human and the application interaction to a wav file.&#8221; Again, this is probably very useful for analyzing application performance and the accuracy of recongitions, but it won&#8217;t work in this example.

**A Subdialog for Confidence-Based Recognition**

Structuring the confidence-based recognition example as a subdialog looks something like the example below.

<pre>&lt;?xml version=&ldquo;1.0&ldquo; ?&gt;
&lt;!DOCTYPE vxml PUBLIC &ldquo;-//BeVocal Inc//VoiceXML 2.0//EN&ldquo;
  &ldquo;http://cafe.bevocal.com/libraries/dtd/vxml2-0-bevocal.dtd&ldquo;&gt;
&lt;vxml version=&ldquo;2.0&ldquo; xmlns=&ldquo;http://www.w3.org/2001/vxml&ldquo;&gt;

&lt;!-- Event handler for noinput and nomatch events --&gt;
&lt;catch event=&ldquo;noinput nomatch&ldquo;&gt;
I'm Sorry. I didn't get that.
&lt;reprompt/&gt;
&lt;/catch&gt;

&lt;!-- Threshold for confidence-based recognition--&gt;
&lt;var name=&ldquo;thresh&ldquo; expr=&ldquo;0.75&ldquo;/&gt;

&lt;!-- Enable audio capture --&gt;
&lt;property name=&ldquo;recordutterance&ldquo; value=&ldquo;true&ldquo;/&gt;

&lt;form id=&ldquo;choose&ldquo;&gt;

&lt;field name=&ldquo;main&ldquo;&gt;
&lt;prompt&gt;Please select the candidate you want to vote for?&lt;/prompt&gt;

&lt;grammar src=&ldquo;voteGrammar.grxml&ldquo;/&gt;

&lt;filled&gt;
&lt;if cond=&ldquo;main$.confidence & lt; thresh &ldquo;&gt;

&lt;subdialog name=&ldquo;sure&ldquo; src=&ldquo;makeSure.vxml&ldquo;&gt;
  &lt;param name=&ldquo;vote&ldquo; expr=&ldquo;main$.utterance&ldquo;/&gt;
&lt;/subdialog&gt;
 &lt;if cond=&ldquo;sure.isRight=='true'&ldquo;&gt;
 &lt;log&gt;Voter confirmed selection for recongition less than
 &lt;value expr=&ldquo;100*(thresh)&ldquo;/&gt; percent.&lt;/log&gt;
 &lt;else/&gt;
 &lt;clear namelist=&ldquo;main&ldquo;/&gt;
 &lt;goto nextitem=&ldquo;main&ldquo;/&gt;
 &lt;/if&gt;
&lt;/if&gt;
&lt;prompt&gt;Thank you.&lt;/prompt&gt;
&lt;submit next=&ldquo;process.php&ldquo; namelist=&ldquo;main&ldquo; method=&ldquo;post&ldquo; enctype=&ldquo;multipart/form-data&ldquo;/&gt;
&lt;/filled&gt;

&lt;/field&gt;
&lt;/form&gt;

&lt;/vxml&gt;
</pre>

There are several important elements here. First, note that I use a stand alone variable to declare the level of confidence that I&#8217;ll be requiring on certain fields. This will make it easier to change the value across multiple fields if testing shows that a differnet confidence level is needed.

Within the <filled> element (which lets us inspect the result of a successful recognition on a field variable) we test to see whether the confidence level of the recognition meets our minimum threshold. If it does not meet our minimum threshold, we call the subdialog contained in the file &#8220;makeSure.vxml&#8221; and pass it a string representing the recognized utterance. (Keep in mind, the platform&#8217;s default recognition level is also at work here &#8211; if the confidence level of the recognition falls below this default threshold, a nomatch event will be thrown. For our test to be applied, the confidence level of the recognition must be at least this default threshold level.)

Without looking at the specifics of the subdialog just yet, we can see that if a value of &#8216;true&#8217; is returned, we&#8217;ll log that the voter has confirmed their selection and we&#8217;ll then send their vote on for further processing. If any other value is returned from our subdialog, we&#8217;ll clear the field variable and start again. (The <goto> element in this part of the application may be a bit redundant &#8211; the Form Interpretation Algorithm should ensure another visit to the field called &#8220;main&#8221; after its value has been cleared.)

Now lets have a look at the subdialog that confirms a voter&#8217;s selection:

<pre>&lt;?xml version=&ldquo;1.0&ldquo; ?&gt;
&lt;!DOCTYPE vxml PUBLIC &ldquo;-//BeVocal Inc//VoiceXML 2.0//EN&ldquo;
  &ldquo;http://cafe.bevocal.com/libraries/dtd/vxml2-0-bevocal.dtd&ldquo;&gt;
&lt;vxml version=&ldquo;2.0&ldquo; xmlns=&ldquo;http://www.w3.org/2001/vxml&ldquo;&gt;

&lt;form id=&ldquo;makeSure&ldquo;&gt;
&lt;var name=&ldquo;vote&ldquo;/&gt;
&lt;var name=&ldquo;isRight&ldquo;/&gt;

 &lt;field name=&ldquo;confirm&ldquo; type=&ldquo;boolean&ldquo;&gt;
 &lt;noinput&gt;I'm Sorry.  I did not hear what you said.&lt;reprompt/&gt;&lt;/noinput&gt;
 &lt;nomatch&gt;&lt;reprompt/&gt;&lt;/nomatch&gt;
   &lt;prompt&gt;I think I heard you vote for, &lt;value expr=&ldquo;vote&ldquo;/&gt;.  Is this correct?&lt;/prompt&gt;

   &lt;filled&gt;
    &lt;if cond=&ldquo;confirm&ldquo;&gt;
    &lt;assign name=&ldquo;isRight&ldquo; expr=&ldquo;'true'&ldquo;/&gt;
    &lt;else/&gt;
    &lt;assign name=&ldquo;isRight&ldquo; expr=&ldquo;'false'&ldquo;/&gt;
    &lt;/if&gt;
    &lt;return namelist=&ldquo;isRight&ldquo;/&gt;
   &lt;/filled&gt;

 &lt;/field&gt;
&lt;/form&gt;

&lt;/vxml&gt;
</pre>

As you can see, this is a simple form that takes in the parameter passed from the previous dialog (&#8220;vote&#8221;). It uses a very simple boolean grammar (one of the [builtin grammar types in VoiceXML](http://www.w3.org/TR/voicexml20/#dmlABuiltins)) to confirm a voter&#8217;s selection. If a voter confirms the value passed to the subdialog, the variable &#8220;isRight&#8221; is set to &#8216;true&#8217; and returned to the VoiceXML page that referenced the subdialog. Otherwise, the value of this variable is set to &#8216;false&#8217;.

What I like about this approach is that it makes it pretty straightforward to apply a higher level of confidence to specific fields within an application simply by applying a quick test on each successful recognition and making a reference to the subdialog. You don&#8217;t have to rewrite or copy and past a bunch of lines in different fields within the application.

Now lets look at our simple processing script &#8211; this is the document that is referenced in our main dialog that receives the value of the voters input after a sufficiently confident recognition has occurred.

<pre>&lt;?php

// Get posted vote value
$_vote = htmlspecialchars($_REQUEST['main']);

echo '&lt;?xml version=&ldquo;1.0&ldquo;?&gt;';
echo &ldquo;n&ldquo;;

?&gt;
&lt;!DOCTYPE vxml PUBLIC &ldquo;-//BeVocal Inc//VoiceXML 2.0//EN&ldquo;
  &ldquo;http://cafe.bevocal.com/libraries/dtd/vxml2-0-bevocal.dtd&ldquo;&gt;
&lt;vxml application=&ldquo;confidenceTest.vxml&ldquo; version=&ldquo;2.0&ldquo; xmlns=&ldquo;http://www.w3.org/2001/vxml&ldquo;&gt;

&lt;form id=&ldquo;playBack&ldquo;&gt;
&lt;block&gt;
&lt;prompt&gt;
You voted for &lt;?php echo $_vote ?&gt;, &lt;audio expr=&ldquo;application.lastaudio$&ldquo;/&gt;.
&lt;break size=&ldquo;small&ldquo;/&gt;
Goodbye.
&lt;/prompt&gt;
&lt;disconnect/&gt;
&lt;/block&gt;
&lt;/form&gt;

&lt;/vxml&gt;
</pre>

What&#8217;s important to note here is that I did not submit the audio of the captured utterance to the processing script. In order to do this, you need to use the [VoiceXML 2.1 <data> tag](http://www.w3.org/TR/2005/CR-voicexml21-20050613/#sec-data). However, the submission of captured audio through the <data> tag will not perform a transition to a new dialog, something we want to do to tell the voter who they selected (or, alternatively, that their vote was processed). So, I&#8217;ll take a bit of a different approach here and reference the variable &#8220;application.lastaudio$&#8221; &#8211; I also make a reference to our original dialog in the &#8220;application&#8221; attribute of the <vxml> element.

The &#8220;application.lastaudio$&#8221; variable is a BeVocal extension that &#8220;contains an audio capture of the user&#8217;s speech&#8221; when it &#8220;matches a field grammar, a form grammar, a menu choice, or a link grammar.&#8221; Obviously this won&#8217;t work on other platforms, but as support for the VoiceXML 2.1 specification improves this approach can be modified to be platform independent.

**Conclusion**

Confidence-based recognition along with some of the new features of the VoiceXML 2.1 specification provide a very powerful mechanism for confirming user selections in voice applications. This type of approach is essential for application like [telephone-based voting systems](http://www.voiceingov.org/blog/?cat=5).

If you would like all of the files in this example (along with the grammar file used, but not discussed, here). You can [get them all here](../xfiles/tutorials/confidence_example.zip). Any suggestions for improvement are welcome.

<!-- Creative Commons License -->

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/"><img alt="Creative Commons License" border="0" src="http://creativecommons.org/images/public/somerights20.gif" /></a>
  
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/">Creative Commons Attribution-NonCommercial-ShareAlike 2.5 License</a>.

<!-- /Creative Commons License -->