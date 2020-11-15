---
id: 37
title: A Lament for Stranded Data
date: 2005-07-07T09:34:17+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=37
permalink: /a-lament-for-stranded-data/
categories:
  - Standards
---
The State of Rhode Island recently deployed a [cluster of XML feeds](http://www.sec.state.ri.us/govtracker/services/) providing access to information ranging from membership on state boards and commissions, corporate information, information on registered lobbyists and a host of other information. This new service, billed as "GovTracker" by the Rhode Island Secretary of State's Office (the entity that developed it) is aimed at opening up access to government information to Rhode Island citizens.

There is a lot here that should make voice applications developers (and other developers) very happy. VoiceXML has a [standard mechanism](http://www.w3.org/TR/2005/CR-voicexml21-20050613/#sec-data) for exposing data stored in XML documents for use in voice applications. Additionally, several platforms have implemented extensions to allow for the [integration of web services](http://www.voiceingov.org/blog/?p=27) into voice applications (although I don't believe any support the REST-style services deployed by Rhode Island).

In announcing the deployment of these services, officials in the Rhode Island Secretary of State's Office observe that:

> [G]overnment technology often quarantines its data from other agencies and its own citizens. While sensitive government data must be protected, there are many ways that citizens would be better served by making specific public content available through open services.

I agree strongly with this statement, and that compels me to comment on what I think is bad about this announcement. It completely ignores the most obvious, most simple, low-cost and highly effective steps that governments can take to make their data and information more accessible to citizens.

As interesting and valuable as the new Rhode Island GovTracker services are, the information they expose represents a tiny sliver of the government data "pie." To be perfectly clear, most of the information that governments publish to the web is stranded in legacy HTML formats. To paraphrase the official quoted above, this information is hidden in plain site. You can (usually) see it with a visual web browser, but that's about it. Sometimes even that's a crapshoot.

Not only is the information and data contained in these government web pages not in XML format, it often does not conform to the legacy HTML specifications they intend to use. This doesn't have to be "“ [XHTML](http://www.w3.org/TR/xhtml1/) (the new and updated HTML standard that conforms to the structure and syntax rules of XML) is well understood and easy to implement. [Contemporary web browsers](http://www.webstandards.org/learn/reference/prolog_problems.html) can understand and render properly formatted XHTML without issue. Unfortunately, it appears that updating the huge mass of information the governments have published from HTML to XHTML is a lot less sexy than deploying new web services.

Often the information in question is highly relevant to citizens. By way of example, check out the Governor of the State of Rhode Island's web site for a [listing of Executive Orders](http://www.governor.ri.gov/executiveorders/). Oftentimes, orders issued by a state's Chief Executive have a direct impact on the lives of state residents. Yet, despite the relevance if this information, the page is constructed with a legacy HTML format. Making matters worse, it [fails to conform](http://validator.w3.org/check?verbose=1&uri=http%3A//www.governor.ri.gov/executiveorders/) to even that standard. Even the Secretary of State's web page listing open meetings [fails to validate](http://validator.w3.org/check?verbose=1&uri=http%3A//www.sec.state.ri.us/pubinfo/openmeetings/upcoming_meetings.html) (although it is built using the XHTML standard).

Try using [XSLT](http://www.w3.org/Style/XSL/) (or any other [XML processing technology](http://us2.php.net/simplexml)) to convert the information on these pages to [XHTML+Voice](http://www.voiceingov.org/blog/?page_id=13), WAP or any other XML format and you're out of luck "“ they either aren't built using XML or they aren't valid. Surely the correction of this problem is a lofty goal that state governments can aspire to.