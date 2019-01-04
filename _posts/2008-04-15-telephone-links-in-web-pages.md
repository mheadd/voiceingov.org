---
id: 133
title: Telephone Links in Web Pages
date: 2008-04-15T09:26:37+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=133
permalink: /telephone-links-in-web-pages/
original_post_id:
  - "133"
categories:
  - Standards
---
Have you ever hovered over a hyperlink on a web page and noticed that it uses the mysterious **tel:** protocol? What happens when you click on a link [like this](tel:18005551234) in your browser?

The answer to that question can depend a lot on how your browser is set up. If your using Firefox, you can associate specific applications with different protocols by opening a new tab and entering **about:config** in the address bar.

In the configuration settings you&#8217;ll need to add two entries, or modify the existing ones if these are already there:

  * network.protocol-handler.external.tel (boolean &#8211; set to true).
  * network.protocol-handler.app.tel (string &#8211; the path to the application you want to launch)

Not exactly sure if this works the same way on Windows, but on my Linux machine I set the second value to the path to my Gizmo softphone (/usr/bin/gizmo) and it automatically launches and dials the number. Pretty neat!

There is a <a href="http://www.faqs.org/rfcs/rfc3966.html" target="_blank">standard for telephone URIs</a> out there, but there doesn&#8217;t seem to be a lot of good information on how different browsers / platforms deal with this type of link.

This might be a good area for some standards, especially since lots of web content is now being consumed on mobile devices (like iPhones) that are more tightly coupled with phones.

If anyone is aware of some standards in this area, or even where better information on different browsers / platforms can be obtained, please [call me](tel:13024824592). 😉