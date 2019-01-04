---
id: 2045
title: Building Voice Applications with Tropo and Node.js
date: 2010-10-01T15:46:51+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=2045
permalink: /building-voice-applications-with-tropo-and-nodejs/
jd_tweet_this:
  - 'no'
original_post_id:
  - "2045"
categories:
  - Development Tools
  - Open Source
---
I&#8217;ve been smitten of late with [Node.js](http://nodejs.org/).

Node.js is a framework for building server-based applications in JavaScript. Node.js is event driven, so if you got a fairly good understanding of state-driven development frameworks you&#8217;ll probably get it quickly. If not, start [here](http://debuggable.com/posts/understanding-node-js:4bd98440-45e4-4a9a-8ef7-0f7ecbdd56cb).
  
<img src="http://localhost:8000/wp-content/uploads/2010/10/nodejs_.jpg" alt="Node.js" title="Node.js" width="222" height="75" style="float:right;margin:15px;" />
  
I wanted to learn more about Node.js, so I decided to build a module. There are lots of modules out there, but I wanted to do something very specific with mine. I wanted to use Node.js to build voice applications. (Not a shocker, it&#8217;s what I do.)

Turns out Node.js is a very nice match for the [Tropo WebAPI](https://www.tropo.com/docs/webapi/), a cloud-based API for building sophisticated speech and communication applications. The Tropo WebAPI speaks [JSON](http://json.org/), and I can&#8217;t think of any more natural way of creating and consuming JSON than with good &#8216;ol JavaScript. Really, you can see why this gets me excited.

The Node.js module that I&#8217;ve been working on for interacting with the Tropo WebAPI is now [available on GitHub](http://github.com/tropo/tropo-webapi-node). It comes with some very nice examples, and even a set of unit tests (yes, Virginia, you can write unit tests with Node.js). It has everything you need to get started using Node.js to write voice apps in JavaScript.

If you decide to give it a try (which I hope you do), there are some additional ingredients I would recommend adding to the mix:

  * The [Express.js framework](http://expressjs.com/) &#8211; a Node.js module very much like [Sinatra](http://www.sinatrarb.com/) in Ruby or [Limonade](http://www.limonade-php.net/) in PHP.
  * [CouchDB](http://couchdb.apache.org/) &#8211; The wonderfully powerful document-oriented storage engine that uses both JavaScript (for map/reduce and views) and JSON (for storing documents). There are also many fine [Node.js modules](http://github.com/ry/node/wiki/modules) available for interacting with CouchDB.

With these ingredients you&#8217;ve got a pretty powerful foundation on which to build robust, sophisticated multi-channel communication apps.

But why would you want to build a voice application with JavaScript?
  
<img src="http://localhost:8000/wp-content/uploads/2010/03/tropo.jpg" alt="Tropo" title="Tropo" width="194" height="119" style="float:left;margin:15px;" />
  
Pretty much all of the voice application development tools and technologies that have been developed over the last decade or so have one essential unifying characteristic &#8211; each of them seeks to leverage easy to understand, low cost web technologies to build phone applications.

This principle can be seen very clearly in the approach embodied by the new [Node.js library for the Tropo WebAPI](http://github.com/tropo/tropo-webapi-node). If you can write JavaScript, you can build sophisticated, cloud-based communication applications that not long ago required specialized skills, training, software and hardware (Big bucks, people. Big bucks).

Cloud-based telephony services based around simple to use APIs that employ widely supported standards like HTTP and JSON are democratizing phone and voice application development.

It&#8217;s really exciting to be a part of this trend and to contribute tools that others can use to build powerful applications.