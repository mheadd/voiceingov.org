---
id: 21
title: Adventures in Client Side Scripting
date: 2005-06-19T16:54:13+00:00
author: mheadd
layout: page
guid: http://www.voiceingov.org/blog/?page_id=21
original_post_id:
  - "21"
---
**Increased Functionality = Increased Complexity?**

Sophisticated VoiceXML applications, like their visual counterparts rely heavily on information stored in back-end databases. Such applications may be executed in an application environment where the code that is consumed by the VoiceXML interpreter is generated at run time. It is quite feasible, and sometime very desirable, to engineer VoiceXML applications in this way using programming languages like PHP, ASP and JSP (among others). For example, a VoiceXML application that provides information updates might be designed to query a database at run time to alert the caller to any new updates. The trade off for this level of sophistication is greater complexity and additional points of failure for an application.

To understand these issues better, it helps to have an understanding of the different parts of a dynamic VoiceXML application.

![VoiceXML Application Architecture](../images/vxml_app_arch.JPG)

Clearly, an application designed like this will require components that a more streamlined VoiceXML application would not. In addition to the standard web sever, a run time environment, where application code is executed must be established, as must a database server to process SQL queries. And while all of these components can reside on the same physical server, production performance requirements typically result in these components being placed on separate servers.

As a result, a problem with any one server can negatively impact the application. Other problems, like an error in executing the application code or a problem with the network connecting the different servers can also cause major problems. Unfortunately, if your VoiceXML application is heavily reliant on data stored in a back-end database, there aren&#8217;t a lot of really
  
good options.

**Working on the client**

Distilled to its essence, the real power of VoiceXML is that it extends the traditional web development paradigm to the telephone. This underlying development paradigm includes a host of features that help boost the performance of web applications.For example, storing web content and files in a local cache can speed up the delivery of information to end users. Most users are never even aware of this process, but it is a fundamental feature of visual browsers and traditional web pages/applications.

Visual web application developers have learned to leverage this client-side cache by using client side scripting and (where appropriate) storing look up data on the client as well. This is typically done through the use of JavaScript arrays, which act in a way not unlike flat database files. (There is an excellent overview of multidimensional JavaScript arrays at <http://developer.netscape.com/viewsource/goodman_arrays.html>.) 

This same approach can be used in VoiceXML applications as well. As evidenced by some of the other tutorials on this site, JavaScript is fully incorporated into VoiceXML.

**Multidimensional JavaScript Arrays in VoiceXML**

In order to demonstrate the power of multidimensional arrays in voice applications, we need to identify a suitable example whose functions we will emulate. For this example, I have chosen an application created by the State of Delaware &#8211; the Internet Access Locater (<http://www.state.de.us/dti/vxml.shtml>). This application follows the traditional multi-tier architecture found in most web applications with a separation of presentation, application and database layers.

To be clear, this application works well, and the architectural model used is sound. However, the data that is used by this application changes infrequently. Since, the application utilizes and external VoiceXML interpreter, there is always the potential for a degradation in performances caused by problems with the connection between the VoiceXML interpreter and the underlying application layer. This potential degradation can be avoided by storing the data used by the application on the client side (e.g., the VoiceXML interpreter), instead of in a back end database.

**Caching Data on the Client Side**

In a VoiceXML application, the &#8220;client&#8221; is actually the VoiceXML interpreter which acts as sort of proxy client for a caller&#8217;s telephone. (In the VoiceXML application architecture, it isn&#8217;t really feasible to store information on an end users phone.) In order to cache files on a VoiceXML interpreter, the appropriate HTTP headers need to be sent when the document is fetched. There is an excellent article on the benefits of caching data to improve the performance of VoiceXML applications available at [http://www.voicexmlreview.org/Dec2002/features/vxml\_app\_performance.html](http://www.voicexmlreview.org/Dec2002/features/vxml_app_performance.html)
  
which provides more detail on this point.

**Conclusion**

The powerful combination of multidimensional JavaScript arrays and the caching features of VoiceXML interpreters (when used for the right kinds of data-driven voice applications) can go a long way toward improving performance. This approach is probably not right for every application, particularly if the data being used is updated regularly or changes frequently. But for applications where data does not change with great frequency, it is a viable approach to enhance performance.

A complete re-write of the State of Delaware&#8217;s Internet Access Locater system using client-side VoiceXML and multidimensional JavaScript arrays [can be obtained here](../xfiles/tutorials/mda_cache_vxml.zip). This application is optimized to run on the BeVocal platform, but it could be modified easily to work on other VoiceXML platforms.

<!-- Creative Commons License -->

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/"><img alt="Creative Commons License" border="0" src="http://creativecommons.org/images/public/somerights20.gif" /></a>
  
This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/2.5/">Creative Commons Attribution-NonCommercial-ShareAlike 2.5 License</a>.

<!-- /Creative Commons License -->