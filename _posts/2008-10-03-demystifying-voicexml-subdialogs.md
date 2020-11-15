---
id: 148
title: Demystifying VoiceXML Subdialogs
date: 2008-10-03T13:46:15+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=148
permalink: /demystifying-voicexml-subdialogs/
original_post_id:
  - "148"
categories:
  - Tutorials
---
_Note "“ this post demonstrates the use of VoiceXML subdialogs. The examples below were tested on the Voxeo Prophecy platform. To download the Prophecy software and run these examples locally, go to <a href="http://www.voxeo.com/prophecy/" target="_blank">http://www.voxeo.com/prophecy/</a>._

**What are Subdialogs?**

<a href="http://www.w3.org/TR/2004/REC-voicexml20-20040316/#dml2.3.4" target="_blank">Subdialogs</a> are a powerful, but often misunderstood, part of the VoiceXML specification that can be used to create reusable components for telephone applications.

The official W3C VoiceXML specification defines subdialogs thusly:

> A subdialog is like a function call, in that it provides a mechanism for invoking a new interaction, and returning to the original form. Variable instances, grammars, and state information are saved and are available upon returning to the calling document. Subdialogs can be used, for example, to create a confirmation sequence that may require a database query; to create a set of components that may be shared among documents in a single application; or to create a reusable library of dialogs shared among many applications. 

One of the most confusing aspects of subdialogs is that they run in a completely separate execution context from the dialog that invokes them (the parent dialog). However, once developers get over this conceptual hurdle, the real power of subdialogs becomes apparent.

**Fun with Subdialogs**

One of the things I like most about subdialogs is their reusability. I often find myself in situations where I need to collect input from a caller in several steps that are generally the same (i.e., a series of digits), but each has a specific validation requirement.

For example, in order to process a credit card payment there are several pieces of information that need to be obtained form a caller "“ credit card number, CVV number, expiration date, etc. All have the same common characteristic that they are a series of digits, but all have unique verification requirements. Valid credit card numbers have specific lengths and must pass <a href="http://en.wikipedia.org/wiki/Luhn_algorithm" target="_blank">a &#8220;mod 10&#8221; check</a>. CVV numbers are <a href="http://en.wikipedia.org/wiki/Card_Security_Code" target="_blank">specific lengths</a> for different card types.

As with a typical function call in any programming language, parameters can be passed into a VoiceXML subdialog when it is invoked. These parameters often take the form of a string of text to be read out to the caller, or a number that is used to count actions. However, we also have the option of passing more complex data types into a subdialog call

We can pass JavaScript arrays and custom object into a subdialog, or we can pass some of the native object types in JavaScript. For example, every function that is declared in JavaScript is also an instance of the  <a href="http://developer.mozilla.org/en/Core_JavaScript_1.5_Reference/Global_Objects/Function" target="_blank">JavaScript Function Object</a>. So a JavaScript function that is declared in a parent dialog can be passed into a subdialog as a parameter.

For example, consider the following simple subdialog:

<form id=&#8221;S_1&#8243;>

<!&#8211; Parameters passed into subdialog &#8211;>
  
<var name=&#8221;myPrompt&#8221; />
  
<var name=&#8221;myFunction&#8221; />

<catch event=&#8221;noinput nomatch&#8221;>
  
&nbsp;&nbsp;<prompt>That was not valid input. Try again.</prompt>
  
&nbsp;&nbsp;<reprompt/>
  
</catch>

<field name=&#8221;getInput&#8221; type=&#8221;digits&#8221;>
  
&nbsp;<prompt><value expr=&#8221;myPrompt&#8221;/></prompt>
  
&nbsp;&nbsp;<filled>
  
&nbsp;&nbsp;&nbsp;&nbsp;<if cond=&#8221;myFunction(getInput)&#8221;>
  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<return namelist=&#8221;getInput&#8221;/>
  
&nbsp;&nbsp;&nbsp;&nbsp;<else/>
  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<clear/>
  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<throw event=&#8221;nomatch&#8221;/>
  
&nbsp;&nbsp;&nbsp;&nbsp;</if>
  
&nbsp;&nbsp;</filled>
  
</field>

This subdialog accepts two parameters "“ a prompt that is read out to the caller, and a function that is used to validate the input. The conditional logic in the <filled> block assumes that the function returns a boolean (true/false).

This same subdialog structure can be used to collect input that meets a wide range of validation criteria. To use this subdialog to collect a 5-digit zip code, we would set the parameters as follows:

> // JavaScript function to determine length of a string variable
  
> function isFive(x) {
  
> &nbsp;&nbsp;if (x.length == 5 ) {
  
> &nbsp;&nbsp;&nbsp;&nbsp;return true;
  
> &nbsp;&nbsp;}
      
> return false;
    
> }
> 
> <param name=&#8221;myPrompt&#8221; expr=&#8221;&#8216;Please enter your five digit zip code.'&#8221;/>
  
> <param name=&#8221;myFunction&#8221; expr=&#8221;isFive&#8221;/> 

To use this subdialog to collect and validate a credit card numbers, we would set the parameters as follows:

> // JavaScript function to perform a mod 10 check
  
> function mod10Check(x) {
  
> &nbsp;&nbsp;&nbsp;&nbsp;// logic of mod 10 check
  
> &nbsp;&nbsp;&nbsp;&nbsp;return true;
      
> }
      
> return false;
    
> }
> 
> <param name=&#8221;myPrompt&#8221; expr=&#8221;&#8216;Please enter your credit card number.'&#8221;/>
  
> <param name=&#8221;myFunction&#8221; expr=&#8221;mod10Check&#8221;/> 

A sample VoiceXML document, demonstrating how this same basic subdialog can be used to collect different kinds of input <a href="../tutorials/sub_db_example.vxml.txt" target="_blank">can be found here</a>.

As this discussion shows, subdialogs are a tremendously powerful tool that can enhance the reusability of code and reduce the maintenance requirements of telephone applications. So, if you find yourself in a situation where you need to collect the same type of input from a caller several times during a call flow, take a look at subdialogs.

Their power and reusability have the potential to make your life a lot easier.