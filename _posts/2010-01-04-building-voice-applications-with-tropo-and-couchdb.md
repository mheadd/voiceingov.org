---
id: 1471
title: Building Voice Applications with Tropo and CouchDB
date: 2010-01-04T19:49:20+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1471
permalink: /building-voice-applications-with-tropo-and-couchdb/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] Building Voice Applications with Tropo and CouchDB. #url# #voxeo #tropo #couchdb #ivr'
wp_jd_target:
  - http://www.voiceingov.org/blog/?p=1471
wp_jd_clig:
  - http://cli.gs/sTZ0X
original_post_id:
  - "1471"
categories:
  - CouchDB
  - Development Tools
  - Open Source
  - Tutorials
tags:
  - Cloud Telephony
  - Tropo
  - Ubuntu
---
The beginning of a new decade is usually the time when there is a lot of reflection on what has changed for the better (and the worse) over the past 10 years.
  
<img src="http://voiceingov.files.wordpress.com/2010/01/tropo_logo.png" alt="Tropo" title="Tropo" style="margin:10px;padding:15px;float:right;" />

The end of 2009 saw its fair share of pundits talking about how far we&#8217;ve come since the year 2000, but for me the most dramatic change has been to the way that voice applications are developed and deployed. In the past 10 years (hell, in the past 10 months!) we&#8217;ve seen a dramatic shift toward cloud telephony with the launch of a number of new services that have fundamentally altered how voice applications can be built.

There has simply never been a more varied and powerful array of tools available for developers to build phone applications with than exists today. Some of the newest and most innovative platforms around for building cloud-based phone applications are listed below:

  * <a href="http://www.tropo.com/" target="_blank">Tropo</a>
  * <a href="http://cloudvox.com/" target="_blank">Couldvox</a>
  * <a href="http://www.callfire.com/dialer/cm/custom_telephony.html" target="_blank">CallFire</a>
  * <a href="http://tringme.com/" target="_blank">TringMe</a>
  * <a href="http://www.twilio.com/" target="_blank">Twilio</a>
  * <a href="http://www.ribbit.com/" target="_blank">Ribbit</a>

Over the same period of time there&#8217;s also been lots of changes to some of the foundational technologies supporting voice applications (and other kinds of web applications). The NoSQL movement is gaining steam, and there is an interesting collection of new <a href="http://nosql-database.org/" target="_blank">document-oriented databases</a> available for developers to use. One of my favorite is Apache <a href="http://couchdb.apache.org/" target="_blank">CouchDB</a>.

![Apache CouchDB Logo](http://localhost:8000/wp-content/uploads/2010/01/couchdb-logo.png)

The thing I find really interesting about CouchDB is that it makes use of an HTTP-based API &#8211; pretty much any tool or technology that can communicate via HTTP can be used to interact with a CouchDB instance (hello command line&#8230;). In addition, that data structure of the documents stored in a CouchDB instance is <a href="http://www.json.org/" target="_blank">JSON</a>. In my mind, this makes CouchDB a very useful choice when building cloud applications, specifically cloud telephony applications.

Who wants to use one of those old relational databases to build a cutting edge cloud phone app? That&#8217;s so 2009.

This post and the next several that follow it will detail how to set up a CouchDB instance and to build a cloud telephony application with it using the Voxeo <a href="http://tropo.com/" target="_blank">Tropo platform</a>.

For those that don&#8217;t know, Tropo is one of the new cloud telephony platforms that lets developers author voice apps quickly and easily using one of several different languages. It&#8217;s open source, well documented and supported by Voxeo&#8217;s industry leading telephony infrastructure.

So, if you want to start the new decade of right by learning how to build powerful, scalable, full featured voice applications using Tropo and CouchDB, read on.

**Getting Started with Tropo**

Head on over to <a href="http://tropo.com/" target="_blank">Tropo.com</a> and set up a new account (if you don&#8217;t have one already). Take a little time to review the <a href="http://docs.tropo.com/" target="_blank">documentation for Tropo</a> &#8211; I&#8217;d recommend running through a few of the sample apps if you have time. They&#8217;re fairly self explanatory and provide a solid overview of the different languages that can be used to write a Tropo app &#8211; I&#8217;ll be using PHP for the example application in this series of blog posts, but you can use any language that Tropo supports.

Deploying and testing an application on Tropo is a snap, and you can even deploy a PSTN number for your application (or you can use the Skype calling number automatically provisioned when you create a Tropo app). More on this in the next post.

**Installing CouchDB**

The next step in building our cloud telephony application for the new decade is getting CouchDB up and running. The steps listed below detail how to install CouchDB 0.9 on Ubuntu 8.04 (the long-term support version of Ubuntu). A few points before we get started&#8230;

This specific combination of Ubuntu and CouchDB is my own preference. I typically run Ubuntu 8.04 when I deploy a new virtual server, but you are free to run whatever version you like, or another Linux distro entirely &#8211; its up to you. Depending on the version of Ubuntu you are running, you may be able to get CouchDB 0.9 installed by simply doing `sudo apt-get install couchdb`.

Keep in mind, though, that the HTTP API for CouchDB can change dramatically between versions &#8211; I&#8217;ve noticed some significant changes when going from 0.8 to 0.9 &#8211; the discussion here will focus on version 0.9 (as does a lot of <a href="http://books.couchdb.org/relax/" target="_blank">good documentation on CouchDB</a> available on the web).

If you don&#8217;t want to install CouchDB yourself, you may be able to take advantage of one of the growing number of CouchDB hosting services like <a href="http://cloudant.com/" target="_blank">CloudAnt </a>or <a href="http://hosting.couch.io/" target="_blank">Couch.io</a>. Again, its up to you.

To install CouchDB 0.9 on Ubuntu 8.04, using the following steps.

Step 1. Determine what version of Ubuntu is running on your machine:

> $ cat /etc/lsb-release

Step 2. Install Erlang &#8211; CouchDB 0.9 requires at least Erlang version 5.5.5. If you are running Ubuntu 8.10 or above, you can probably get the required Erlang version by simply doing `sudo apt-get install erlang`. (Note &#8211; The last step in this section may take a while, feel free to go grab a cup of Joe while the source compiles.)

> $ sudo apt-get build-dep erlang
      
> $ sudo apt-get install java-gcj-compat java-gcj-compat-dev
      
> $ wget http://www.erlang.org/download/otp\_src\_R12B-5.tar.gz
      
> $ tar -zxvf otp\_src\_R12B-5.tar.gz
      
> $ cd otp\_src\_R12B-5/
      
> $ ./configure && make && sudo make install 

Step 3. Install CouchDB dependencies:

> $ sudo apt-get install libmozjs-dev libicu-dev libcurl4-openssl-dev

Step 4. Download CouchDB 0.9 Source and install:

> $ wget http://apache.mirrors.redwire.net/couchdb/0.9.0/apache-couchdb-0.9.0.tar.gz
      
> $ tar -zxvf apache-couchdb-0.9.0.tar.gz
      
> $ cd apache-couchdb-0.9.0/
      
> $ ./configure && make && sudo make install 

Step 5. Create a user for CouchDB (More on this in the CouchDB README.txt file):

> $ sudo adduser &ndash; &ndash;system &ndash; &ndash;home /usr/local/var/lib/couchdb &ndash; &ndash;no-create-home &ndash; &ndash;shell /bin/bash &ndash; &ndash;group &ndash; &ndash;gecos &#8220;CouchDB Administrator&#8221; couchdb
      
> $ sudo chown -R couchdb /usr/local/etc/couchdb
      
> $ sudo chown -R couchdb /usr/local/var/lib/couchdb
      
> $ sudo chown -R couchdb /usr/local/var/log/couchdb 

Step 6. Start CouchDB

> $ sudo /usr/local/etc/init.d/couchdb start
> 
> If you see an error that says:
> 
> _&#8220;Apache CouchDB needs write permission on the PID file: /usr/local/var/run/couchdb.pid&#8221;_
> 
> Do the following, then try starting CouchDB again:
> 
> $ sudo touch /usr/local/var/run/couchdb.pid
      
> $ sudo chown couchdb:couchdb /usr/local/var/run/couchdb.pid
> 
> When CouchDB starts successfully, you will see a message that says:
> 
> *** Starting database server couchdb&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ OK ]** 

Step 7. Test connectivity to CouchDB:

> $ curl http://127.0.0.1:5984
> 
> You should see:
> 
> **{&#8220;couchdb&#8221;:&#8221;Welcome&#8221;,&#8221;version&#8221;:&#8221;0.9.0&#8243;}** 

The configuration file for CouchDB is located at /usr/local/etc/couchdb/local.ini &#8212; in the next post, we&#8217;ll modify some of the config settings for our CouchDB instance so that we can access it via HTTP from the Tropo environment.

We&#8217;ll also set up our first CouchDB database, add some documents and start coding our new cloud telephony application using Tropo.

Stay tuned&#8230;