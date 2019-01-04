---
id: 1304
title: Drupal Notifications Framework
date: 2009-10-27T11:35:48+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1304
permalink: /drupal-notifications-framework/
jd_tweet_this:
  - 'no'
original_post_id:
  - "1304"
categories:
  - Open Source
---
I&#8217;m experimenting with the very cool <a href="http://drupal.org/node/252592" target="_blank">Notifications Framework</a> for Drupal on a test site that I have set up.

This module (really a collection of different modules and add ons for Drupal) adds some extremely powerful functionality to a Drupal site to allow site administrators to select different node types for subscriptions, and to set up different subscription channels. I&#8217;ve currently got <a href="http://drupal.org/project/prowl" target="_blank">Prowl</a> and e-mail working, and am configuring <a href="http://drupal.org/project/twitter" target="_blank">Twitter</a> and <a href="http://drupal.org/project/xmppframework" target="_blank">XMPP</a> for testing now &#8212; I hope to get to <a href="http://drupal.org/project/smsframework" target="_blank">SMS</a> later this week. It also makes it easy for site users to subscribe to different nodes, to be alerted when they are updated or if a comment is submitted. Users can also select the channel they want to be notified on (a default setting or a custom selection for a particular node).

It doesn&#8217;t look like a user can select multiple channels to be notified on (e.g., Prowl and XMPP for the same noticeable event). There also isn&#8217;t currently a phone-based channel (i.e., outbound phone call with <acronym title="Text-to-Speech">TTS</acronym>), at least not that I could easily find. I may have to remedy that soon&#8230;

This is but one example of the incredible array of custom modules available from the <a href="http://drupal.org/project/Modules" target="_blank">Drupal community</a> that Drupal-powered sites can take advantage of.

Be nice to see some sort of multi-channel notification functionality on the new Drupal-powered <a href="http://www.whitehouse.gov/" target="_blank">whitehouse.gov</a> site. Dont think they have that yet.