---
id: 611
title: Extension Methods vs. Prototype
date: 2009-01-21T17:50:09+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=611
permalink: /extension-methods-vs-prototype/
original_post_id:
  - "611"
categories:
  - Development Tools
  - Tutorials
---
I had my first exposure recently to <a href="http://msdn.microsoft.com/en-us/library/bb383977.aspx" target="_blank">extension methods</a> in C# (which I am still relatively new to):

> Extension methods enable you to &#8220;add&#8221; methods to existing types without creating a new derived type, recompiling, or otherwise modifying the original type. Extension methods are a special kind of static method, but they are called as if they were instance methods on the extended type. For client code written in C#&#8230;there is no apparent difference between calling an extension method and the methods that are actually defined in a type. 

This makes it easy to add functionality to existing types &#8212; this comes in pretty handy if you find yourself doing the same thing a number of times in your code. A good example of extension methods in action can be <a href="http://weblogs.asp.net/dwahlin/archive/2008/01/25/c-3-0-features-extension-methods.aspx" target="_blank">found here</a>.

I use a similar approach to format telephone numbers for readback in <acronym title="Text-to-Speech">TTS</acronym> when I need to render VoiceXML using C#/ASP.NET. So, if I have a string representing a telephone number, it may be formatted as 555-111-3333, or (555) 111-3333, or 555.111.3333, etc. When I render a phone number in VoiceXML (using the SSML `<say-as>` tag) I like to use a string of numbers only, no special delimiters or other characters, to ensure it is read out properly by the TTS engine. Extension methods can help with this.

**C# example:**
  
`<br />
public&nbsp;static&nbsp;class&nbsp;StringExtensionsClass<br />
&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;static&nbsp;string&nbsp;GetOnlyNumbers(this&nbsp;string&nbsp;s)<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MatchCollection&nbsp;col&nbsp;=&nbsp;Regex.Matches(s,&nbsp;"[0-9]");<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;StringBuilder&nbsp;sb&nbsp;=&nbsp;new&nbsp;StringBuilder();<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;foreach&nbsp;(Match&nbsp;m&nbsp;in&nbsp;col)&nbsp;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sb.Append(m.Value);&nbsp;<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;sb.ToString();<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;&nbsp;&nbsp;}`

In ASP.NET, you would invoke this extension method as if it were a built in method of String:

> `<br />
string telephoneString = "(555) 111-3333";<br />
...<br />
Response.Write("<say -as interpret-as="telephone">" + telephoneString.GetOnlyNumbers() + "</say-as>");<br />
` 

Extension methods remind me a lot of the <a href="https://developer.mozilla.org/en/Core_JavaScript_1.5_Guide/Class-Based_vs._Prototype-Based_Languages" target="_blank">prototype-based approach</a> of extending classes in JavaScript.

**JavaScript example:**
  
`<br />
String.prototype.getNumbersOnly&nbsp;=&nbsp;function()<br />
{<br />
var&nbsp;mySplitString&nbsp;=&nbsp;this.split("");<br />
var&nbsp;myMatch&nbsp;=&nbsp;new&nbsp;RegExp("[0-9]");<br />
var&nbsp;myNumberString&nbsp;=&nbsp;"";<br />
&nbsp;&nbsp;for(var&nbsp;i=0;&nbsp;i<mysplitstring.length;&nbsp;i++)&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;if(myMatch.test(mySplitString[i]))&nbsp;{<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;myNumberString&nbsp;+=&nbsp;mySplitString[i];<br />
&nbsp;&nbsp;&nbsp;&nbsp;}<br />
&nbsp;&nbsp;}<br />
	return&nbsp;myNumberString;<br />
};<br />
` 

In your VoiceXML code, you would invoke this custom method of the JavaScript String Object like this:

> `<br />
<var name="telephoneString" expr="'(555) 111-3333'"/><br />
...<br />
<say -as interpret-as="telephone"><value expr="telephoneString.getNumbersOnly()"/></say-as><br />
` 

So, whether you need to modify your phone number string in your server-side code, or in your client-side code this approach can make the job easy and consistent.