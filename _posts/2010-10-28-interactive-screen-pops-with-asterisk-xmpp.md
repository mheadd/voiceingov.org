---
id: 2145
title: 'Interactive Screen Pops with Asterisk &#038; XMPP'
date: 2010-10-28T16:56:12+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=2145
permalink: /interactive-screen-pops-with-asterisk-xmpp/
original_post_id:
  - "2145"
twitter_cards_summary_img_size:
  - 'a:7:{i:0;i:239;i:1;i:195;i:2;i:1;i:3;s:24:"width="239" height="195"";s:4:"bits";i:7;s:8:"channels";i:3;s:4:"mime";s:9:"image/gif";}'
categories:
  - Asterisk
  - Open Source
---
I&#8217;ve got a thing about [screen pops](http://en.wikipedia.org/wiki/Screen_pop).
  
<img src="/wp-content/uploads/2010/10/asterisk1.gif" alt="Asterisk" title="Asterisk" style="margin:10px;padding:15px;float:right;" />
  
I&#8217;ve [written before](http://www.voiceingov.org/blog/?p=326) about using Asterisk and XMPP to enable IM-based screen pops, but the recent release of Asterisk 1.8 creates a whole new reason to be excited about this topic.

The [new version of Asterisk](http://www.asterisk.org/node/51444) includes a new dialplan function called [JABBER_RECEIVE](https://wiki.asterisk.org/wiki/display/AST/Function_JABBER_RECEIVE).

This new function nicely compliments the existing [JabberSend() dialplan application](https://wiki.asterisk.org/wiki/display/AST/Application_JabberSend) and lets you read incoming XMPP messages into dialplan variables (via [Set()](https://wiki.asterisk.org/wiki/display/AST/Application_Set)).

Now that you can both send **and** receive XMPP messages via the dialplan, it is possible to build sophisticated CTI applications using standards-based XMPP servers and clients with nothing but `extensions.conf`. Here&#8217;s how.

You&#8217;ll need an XMPP server with (at least) two accounts. One for you, as a user. One for Asterisk. You&#8217;ll also want to fire up your XMPP client and add the Asterisk user to your buddy list.

Set up jabber.conf with the details of the Asterisk account on your XMPP server (make sure you run `jabber reload` in the Asterisk CLI after modifying the file):

Once you&#8217;ve done that, you&#8217;ll need to add some dialplan logic to use both JabberSend() and JABBER_RECEIVE (run `dialplan reload` in the Asterisk CLI after adding this logic):

In this simple example, anytime a call comes into the `default` context, a set of IM messages are sent to the XMPP account user@xxx.xxx.xxx.xxx (where xxx.xxx.xxx.xxx represents the host name/IP for your XMPP server). The following line in the dialplan will cause Asterisk to wait 10 seconds to receive a response from user@xxx.xxx.xxx.xxx.

> exten => \_XXXX,n,Set(OPTION = ${JABBER\_RECEIVE(asterisk,user@xxx.xxx.xxx.xxx,10)}) 

When a response is received, it is read into the variable OPTION. Subsequent dialplan logic will either send the call to the extention that was dialed, or simply hang up (you could just as easily add options and logic to route the call to one of several different phone numbers or to voicemail).

That&#8217;s it!

This powerful new addition to Asterisk makes building sophisticated, interactive XMPP-based screen pops easy. Just imagine what other <a href="http://svn.digium.com/view/asterisk/branches/1.8/CHANGES?view=markup" target="_blank">juicy little nuggets</a> await in the new version of Asterisk.

Happy screen popping!