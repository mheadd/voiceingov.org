---
id: 1708
title: Upgrading to CouchDB 0.11
date: 2010-04-07T08:33:47+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1708
permalink: /upgrading-to-couchdb-011/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] Upgrading to CouchDB 0.11. #url# #couchdb #nosql #tropo'
wp_jd_clig:
  - http://cli.gs/u9SqL
wp_jd_target:
  - ""
original_post_id:
  - "1708"
categories:
  - CouchDB
  - Development Tools
  - Open Source
tags:
  - Tropo
  - Ubuntu
---
Just a few days ago, <a href="http://blog.couch.io/post/490755034/couchdb-0-11-0-released" target="_blank">CouchDB version 0.11 was released</a> &#8211; this new version is packed full of cool new features as <a href="http://blog.couch.io/post/468392274/whats-new-in-apache-couchdb-0-11-part-three-new" target="_blank">outlined on the Couch.io blog</a>. It&#8217;s also the first release without the Alpha or Beta label attached to it.
  
![Apache CouchDB Logo](/wp-content/uploads/2010/01/couchdb-logo.png)
  
What&#8217;s more exciting, CouchDB version 0.11 is a feature-freeze release candidate for the upcoming version 1.0. So if you&#8217;ve played around with CouchDB and have an old instance laying around, now is the time to upgrade.

If you&#8217;ve read [my previous series on using CouchDB](http://www.voiceingov.org/blog/?p=1525) to build cloud telephony applications with Voxeo&#8217;s <a href="https://www.tropo.com/home.jsp" target="_blank">Tropo platform</a>, and you used my instructions for setting up CouchDB on Ubuntu 8.04, then upgrading to CouchDB version 0.11 will be a piece of cake. (Note &#8211; the mirror you download from may be different than below. Go to the <a href="http://couchdb.apache.org/downloads.html" target="_blank">download page</a> to find the best one):

Before upgrading, make sure that any customizations you&#8217;ve made to the CouchDB configuration are in `/usr/local/etc/couchdb/local.ini`. The upgrade process will overwrite any changes you have made in `default.ini`.

> $ sudo /usr/local/etc/init.d/couchdb stop
> 
> You should probably run _make uninstall_ on the previous version of CouchDB before starting.
  
> If you see leftover files in _/user/local_
  
> $ find /usr/local -name \*couch\* | wc -l
> 
> You should probably get rid of them:
  
> $ find /usr/local -name \*couch\* | xargs rm -rf
> 
> $ wget http://www.trieuvan.com/apache/couchdb/0.11.0/apache-couchdb-0.11.0.tar.gz
  
> $ tar -zxvf apache-couchdb-0.11.0.tar.gz
  
> $ cd apache-couchdb-0.11.0/
  
> $ ./configure && make && sudo make install
  
> $ sudo /usr/local/etc/init.d/couchdb start
  
> $ curl -X GET http://127.0.0.1:5984
> 
> You should see:
> 
> **{”couchdb”:”Welcome”,”version”:”0.11.0&#8243;}** 

Note &#8211; if you see an error that says `{"error":"error","reason":"eacces"}` when trying to create a database or insert documents, you may need to re run some commands listed in the previous install instructions:

> $ sudo chown -R couchdb /usr/local/etc/couchdb
  
> $ sudo chown -R couchdb /usr/local/var/lib/couchdb
  
> $ sudo chown -R couchdb /usr/local/var/log/couchdb 

I&#8217;m in the process of finishing up a project that will make use of the <a href="http://wiki.open311.org/API" target="_blank">Open311 API deployed by the City of San Francisco</a> and it shall be SQL-free &#8211; now that I have CouchDB 0.11 installed, I&#8217;m ready to finish up.

Stay tuned &#8211; this project is going to kill!