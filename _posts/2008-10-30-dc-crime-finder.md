---
id: 150
title: DC Crime Finder
date: 2008-10-30T15:15:05+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=150
permalink: /dc-crime-finder/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "150"
categories:
  - Cell Phones
  - General Discussion
---
The &#8220;DC Crime Finder&#8221; is a multimodal app that lets residents of the District of Columbia search for crime locations in their neighborhoods.

It uses <a href="http://data.octo.dc.gov/" target="_blank">actual crime data published by the District</a> and supports a wide range of devices for looking up addresses and crime locations. The DC Crime Finder works with traditional desktop web browsers, mobile devices and PDAs, smart phones, iPhones, G1 phones &#8212; essentially any device that has a web browser and access to the Internet.

<img style="float:left;padding:15px;margin:5px;" title="DC Crime Finder" src="http://localhost:8000/wp-content/uploads/2008/10/cell-phonethumbnail.gif" alt="" />

The application also works with ordinary cell phones and even land line phones. It sports a voice user interface (VUI) which makes it accessible from any old school telephone &#8212; even a rotary phone (if anyone in DC still has one).

This application is my submission to the <a href="http://www.appsfordemocracy.org/" target="_blank">Apps For Democracy</a> contest being sponsored by the District of Columbia and iStrategyLabs.

**Get the App:**

The source code for the application is hosted on <a href="http://code.google.com/p/dc-crime-locater/" target="_blank">Google Code</a>.

Its written in PHP and makes use of the <a href="http://www.hawhaw.de/" target="_blank">HAWHAW</a> PHP library by Norbert Huffschmid. This application should run equally well on Linux or Windows (I developed it on Ubuntu 8.04), but you&#8217;ll need PHP 5 with SOAP support enabled.

You&#8217;ll also need a MySQL database &#8212; the District of Columbia provides information on crime locations in a variety of formats, including real time XML-based feeds. For this application, I opted to go with data in CVS format that I imported into a simple MySQL table. One of the things the application does is to calculate the distance of crime locations from a specific address. I believe that this calculation is much more efficiently done in the context of a database, rather than trying to use a real time XML feed (there are a **lot** of crime locations).

The other requirement, if you want to hear this application on a standard telephone or cell phone, is to grab a copy of <a href="http://www.voxeo.com/prophecy/" target="_blank">Voxeo Prophecy</a>. This app should run on most mature VoiceXML/CCXML platforms, but Prophecy is by far the easiest to use and the most standards compliant. Best of all, its free to download and use. If your interested in developing other telephone applications, consider signing up for a <a href="http://evolution.voxeo.com" target="_blank">free developer account</a> with Voxeo.

**Test the App:**

I&#8217;ve set up a demo of this application in a test environment. You can look at the visual interface for this application (which uses a simulator to mimic the look and feel of a cell phone browser) by <del>clicking here</del> (demo is no longer active). Any mobile device with a browser can also access the application &#8212; the HAWHAW library uses some pretty slick device detection, so any device that can handle it should get standard XHTML. Smaller, or older devices will get old school WAP markup.

See <a href="http://www.voiceingov.org/blog/wp-content/app_view_ipaq-228x300.jpg" target="_blank">what this app looks like</a> in an older iPaq handheld device.

The beauty of this application is that when a voice browser (like Voxeo Prophecy) comes a knockin&#8217; it gets its standard markup language &#8211; VoiceXML. If you want to hear this application on a telephone, simply dial <del>(202) 684-7894.</del> (demo is no longer available.)

> _<del>Note: this demo is currently running in a test environment, so you may experience the occasional hiccup. A production deployment of an application like this would be much more robust.</del>_
> 
> **Unfortunately, the demo is no longer active.**

**Why a Multimodal App?**

&#8220;Multimodal Applications&#8221; provide access to services and information through different modalities. This application provides access to crime information, including the ability to search for crime incidents by proximity, through a wide range of different client devices include traditional web browsers, handheld devices and PDAs, cell phones and standard land line telephones.

Unlike other applications that are targeted to a specific platform &#8212; i.e., applications targeted to a desktop web browser, or to a specific handheld device &#8212; the DC Crime Locater can be accessed from a range of different devices. The application is accessible from sophisticated handheld devices like iPhones or G1 phones and from standard home telephones.

This Multimodal paradigm can be used to improve access to other types of government information giving citizens more choices in the devices (and modalities) they will use to consume this information.

**Special Thanks!**

Serious props need to go to the folks in the <a href="http://www.octo.dc.gov/octo/site/default.asp" target="_blank">Office of the CTO</a> in Washington DC. You don&#8217;t see very many government providing the kind of data that the Apps for Democracy challenge is based on. This innovative contest is a fantastic way to get development firms and independent developers (like moi) excited about building powerful applications.

Whether it&#8217;s the U.S Census Bureau, the federal Departments of Labor or Commerce, a state health agency, or a local police force, governments are the repositories of vast amount of information. Much of this data has direct relevance for our everyday lives, and can&#8217;t be obtained from any other source.

How governments make this information available for public consumption will define the debate on open government for many years to come.

I hope other governments follow DC&#8217;s example in putting on this innovative contest.