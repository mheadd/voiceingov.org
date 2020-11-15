---
id: 127
title: Mitigating SQL Injection Vulnerabilities in VoiceXML Applications
date: 2007-12-28T16:24:00+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=127
permalink: /mitigating-sql-injection-vulnerabilities-in-voicexml-applications/
categories:
  - Tutorials
---
[SQL Injection](http://unixwiz.net/techtips/sql-injection.html) is a well known technique for compromising web applications that suffer from vulnerabilities such as improper validation of user input. It allows an attacker to &#8220;inject&#8221; SQL statements into web form inputs that are subsequently used in a database query. When these inputs are not properly sanitized, an attacker can cause all sorts of trouble such as exposing sensitive information or destroying important data.

Most seasoned web developers understand the risk of SQL injection and follow best practices to guard against it. Unfortunately, there are some VoiceXML and IVR developers (I&#8217;ve worked with them!) that do not fully appreciate the risk that SQL injection can pose for voice applications. I&#8217;ve had some strange conversations with these folks, and heard just about every excuse in the book. Here are the usual excuses and my responses to each:

_&#8220;Users access my application through the telephone. You can&#8217;t inject SQL statements into a voice application over the phone&#8230;&#8221;_

VoiceXML platforms communicate with web / application servers using HTTP. Just because most of your users access it through the phone, doesn&#8217;t mean someone can&#8217;t point Firefox at your application server. If your VoiceXML content is served up as straight XML it will display just fine in an ordinary browser (unless you&#8217;ve taken steps to prevent this &#8212; see below). In fact, this is how lots of developers troubleshoot and debug their applications.

_&#8220;There are no links to the CCXML / VoiceXML documents that make up our application, and they aren&#8217;t indexed by search engines. No one can find them.&#8221;_

Ah, the old security by obscurity argument&#8230; Not good enough! There&#8217;s nothing stopping someone from scanning your web server looking for specific documents. If it&#8217;s accessible on your web server, even if you think no one knows where it is, consider it public information.

_&#8220;Our application servers are behind a firewall and aren&#8217;t accessible from outside our network. The only connection to the outside world are the phone lines coming into our call center.&#8221;_

That may be true (its not "“ I assume that there is Internet access in your call center. Doh!), but even if it is it just means your safe from outside attacks. There isn&#8217;t anything stopping anyone from inside your network from getting cute. Still think your safe?

To boil it down, there is no good excuse for not taking steps to avoid SQL injection attacks in voice applications.

### What can be done?

First and foremost, voice developers should have a thorough understanding of the issue and take the appropriate steps to cleanse user input. User input is anything that a user can possibly submit to your application "“ you may have control over what selections a user can make in your voice application, but an attacker that is &#8220;footprinting&#8221; your application with a browser can put in anything they want.

The best way to mitigate the risk of SQL injection attacks against voice applications is (not surprisingly) identical to that for other kinds of web applications. Follow the [checklist provided by Open Web Application Security Project](http://www.owasp.org/index.php/Top_10_2007-A2). If I can quote Brad (Judge Reinhold) from Fast Times at Ridgemont High: &#8220;Learn it&#8230; Know it&#8230; Live it!&#8221;

### Some Additional Considerations for Voice Applications

While it&#8217;s true that voice applications face the same risks as other kinds of web applications when it comes to SQL injection attacks, there are some differences from traditional web applications that can be leveraged to add greater security (or to at least make SQL injection tougher).

Most SQL injection attacks usually begin with the process of &#8220;footprinting&#8221; &#8212; an attacker will usually view the source HTML for a web site and determine what variables are being submitted by a web form, and the URL that they are being submitted to. The attacker will then try and manipulate the values to break the SQL statement that is being constructed with the submitted values. When this happens, valuable information can be gleaned from the behavior of the web application, including any error messages or warnings that are displayed to the end user.

It&#8217;s hard to prevent people from looking at the source code of a web site "“ visual web sites generally have to be available to respond to requests from any browser that requests a page. They work on the standard HTTP port (port 80). Otherwise, visitors would have a hard time seeing and using a web site and it would probably defeat the purpose of building it in the first place.

VoiceXML applications are different. Users do not interact directly with the &#8220;browser&#8221; that is requesting pages from the web server &#8212; they interact with a telephone. A web server that is interacting with a VoiceXML browser does not need to be available to **any** browser making a request "“ it only needs to be available to the VoiceXML browser(s) that it is set up to interact with.

One method of obscuring VoiceXML pages from prying eyes (and reducing the risk of footprinting) is to lock down access to a voice application server, so that it only responds to requests from the IP address(es) of the VoiceXML browser(s) it is set up for. There are several ways to do this:

  * Configure the firewall sitting in front of the server to deny requests from any IP address except those of the VoiceXML browser(s).
  * [Configure the server](http://www.linux-hero.com/howto/network-security-hosts-allow-and-hosts-deny) itself so that only specified IP addresses can access services like http. On Linux this will involve configuring hosts.allow and hosts.deny.
  * Including code in your application to check the IP address of the requesting party to make sure that it is legitimate. Probably not the most efficient or elegant way of doing it, but certainly better than nothing.

In PHP, this last point might look something like:

> if($\_SERVER[&#8216;REMOTE\_ADDR&#8217;] != &#8220;myValidIP&#8221;) {
  
> &nbsp;&nbsp;header(&#8216;HTTP/1.1 404 Not Found&#8217;);
  
> &nbsp;&nbsp;exit();
  
> } 

Another alternative is to [run your web server on an alternate port](http://httpd.apache.org/docs/1.3/mod/core.html#port) "“ there isn&#8217;t any reason that the server responding to requests from your VoiceXML browser needs to run on port 80. (Note "“ this really doesn&#8217;t prevent anyone who knows how to use [netcat](http://netcat.sourceforge.net/) from scanning your server to see what ports are open, but it makes life a bit harder for the average person intent on mischief.)

A final thought on this topic is to limit the requests that your web server will respond to by predicating the response on the MIME types listed in the [HTTP Accept header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). This header tells the web server what kind of content the requesting browser can consume. Many visual web browsers make their requests with the wildcard \*/\* telling the server to send any content back. VoiceXML browsers **should** include the appropriate MIME type for VoiceXML content (&#8220;application/voicexml+xml&#8221;) in their HTTP requests. This would allow someone so inclined to look for this MIME type on incoming requests, and to only consider requests that include it as legitimate.

I&#8217;ll have more on this last point another time. For now, [read up on the checklist](http://www.owasp.org/index.php/Top_10_2007-A2) at the OWASP project. Have a safe and happy New Year!