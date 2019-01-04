---
id: 1510
title: Relaxing on the Couch with Tropo and CouchDB
date: 2010-01-06T19:48:12+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1510
permalink: /relaxing-on-the-couch-with-tropo-and-couchdb/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - '[New Blog Post] Relaxing on the Couch with Tropo and CouchDB. #url# #voxeo #tropo #couchdb #ivr'
wp_jd_target:
  - http://www.voiceingov.org/blog/?p=1510
wp_jd_clig:
  - http://cli.gs/0EPS8
original_post_id:
  - "1510"
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
This is the continuation of a series that will describe how to build voice applications with the <a href="http://www.tropo.com" target="_blank">Tropo</a> cloud telephony platform and <a href="http://couchdb.apache.org/" target="_blank">CouchDB</a>.
  
![Apache CouchDB Logo](http://localhost:8000/wp-content/uploads/2010/01/couchdb-logo.png)
  
In the [last post](http://www.voiceingov.org/blog/?p=1471), I detailed how to get a CouchDB instance up and running on Ubuntu, and how to get an account started on Tropo so that you can start building cloud telephony applications. In this post, we&#8217;ll create our first CouchDB database and create a simple Tropo application that connects to our CouchDB instance. First, however, we need to tweak the default settings for CouchDB so that we can access our CouchDB instance from the an external environment.

**Configuring CouchDB**

Recall from the last post that the configuration files for CouchDB are located in `/usr/local/etc/couchdb/`. Open the local configuration file and take a look at the default settings:

> $ sudo vim /usr/local/etc/couchdb/local.ini 

In the  `[httpd]` section, you’ll notice the setting for the default port that is used to connect to CouchDB &#8211; 5984. You’ll also note the `bind_address` setting. By default, CouchDB listens only on localhost – you can change this by altering the value of `bind_address` to a publicly resolvable IP address (you may need to uncomment this setting as well).

However, before proceeding please note that CouchDB does not yet have a built in security model, so anyone that can access the IP address in the configuration file can potentially access your CouchDB instance. We’ll need to take some steps to restrict access to our CouchDB instance – there are several ways of doing this.

First, if you know the IP address (or range of addresses) that will be accessing your CouchDB instances, you can <a href="https://help.ubuntu.com/community/IptablesHowTo" target="_blank">simply use IPTables to restrict access</a> to that IP:

> $ iptables -A INPUT -p tcp -s 64.57.102.34 &#8211;dport 5984 -j ACCEPT 

The above command would restrict access to your CouchDB instance to a single IP address.

Another method for securing a publicly exposed CouchDB instance is to use Apache password authentication. A good <a href="http://smartic.us/2008/11/08/couchdb-basic-authentication/" target="_blank">overview of this approach</a> can be found here.

After you’ve modified the bind_address setting, restart CouchDB and test connectivity to it:

> $ sudo /usr/local/etc/init.d/couchdb restart
  
> $ curl http://your\_new\_couchdb_ip:5984 

**Creating our CouchDB Database**

Once you have your CouchDB instance up and running, you can create a database in one of two ways. The first, and easiest, is simply to use the `curl` command. You create a database in CouchDB by using the HTTP PUT method:

> $curl -X PUT http://your\_new\_couchdb\_ip:5984/call\_logs 

You can also create a database (and do lots of management functions in CouchDB) by using the GUI (called Futon). Its located at http://your\_new\_couchdb\_ip:5984/\_utils

**Building a Simple Auto Attendant Application with Tropo**

Now that we have an initial database in our CouchDB instance, lets build a simple Tropo application that will populate it with records (or _documents_ in CouchDB parlance):

This simple application is a basic auto attendant. It asks the caller for a 4-digit extension and then transfers them to a 10-digit PSTN number. At the end of the call, we write a very simple call log document to our new call_logs database using the HTTP POST method.

(One small side note – you can use either the POST or PUT methods to insert a document into a CouchDB database. However, using PUT assumes you want to assign a specific document ID to your document. When you use HTTP POST, CouchDB will automatically assign a document ID. For now, we’ll keep things simple and use POST.)

Much of the functionality in this simple app is just stubbed out for now &#8211; i.e., the `getPhoneNumberByExtension()` method &#8211; we’ll build more of this out in later posts.

Modify this file by adding your instance-specifc details to the constant declarations at the top. Do also note that the last two constants can remain blank for now.

> `<br />
define("COUCH_DB_DESIGN_DOCUMENT_NAME", "");<br />
define("COUCH_DB_SHOW_FUNCTION_NAME", "");<br />
` 

We’ll talk about how to use design documents in the next post.

When you load this file up on Tropo and make a test call, you will see your call log document is inserted into the call_logs database. The structure of the document is pure JSON, which is supported <a href="http://us2.php.net/manual/en/book.json.php" target="_blank">quite nicely in PHP</a> (and most every other language that can run on Tropo as well).

In the next post, we’ll examine CouchDB design documents in more detail and modify our simple demo application to get a list of extensions from another CouchDB database and parse the JSON data structure in the `getPhoneNumberByExtension()` method.

Until then, keep on relaxing….