---
id: 544
title: Customizing Caller ID with CiviCRM and trixbox
date: 2009-01-15T18:49:50+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=544
permalink: /customizing-caller-id-with-civicrm-and-trixbox/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "544"
categories:
  - Open Source
  - trixbox
tags:
  - Asterisk
  - CiviCRM
  - PHP
  - VoIP
  - XMPP
---
Recently, I had an opportunity to set up a <a href="http://civicrm.org/" target="_blank">CiviCRM</a> site for a client to use to track contributors.

I think CiviCRM is a great piece of software that can be enormously useful to non-profit organizations, and I&#8217;m actually surprised that more don&#8217;t opt to use it. It&#8217;s powerful, robust, extensible and open source.

The site I set up is running a Red Hat-based <a href="http://www.redhat.com/magazine/003jan05/features/lamp/" target="_blank">LAMP stack</a> with <a href="http://drupal.org/" target="_blank">Drupal</a>, CiviCRM and the <a href="http://civicrm.org/civicontribute" target="_blank">CiviContribute</a> module. I have the same set up mirrored in a local development environment that lets me test tweaks and upgrades to the site before they get pushed out into production. I also have an instance of <a href="http://www.trixbox.org/" target="_blank">trixbox CE</a> running locally, and I decided to try and see if I could set up a trixbox PBX that uses CiviCRM as the data source for looking up caller information on inbound calls. This way, if a non-profit is using both CiviCRM and trixbox they could set their PBX to look up information about clients, donors, volunteers, etc. that call their offices and display this information in either a SIP client or in a [screen pop](http://www.voiceingov.org/blog/?p=326).

The process of setting up trixbox to use CiviCRM as the data source for caller ID lookups couldn&#8217;t be easier (note, I used these steps on trixbox version 2.6.1.10):

<li style="margin-bottom:4px;">
  Log into your trixbox and enter admin mode.
</li>
<li style="margin-bottom:4px;">
  From the toolbar, select <strong>PBX</strong> >> <strong>PBX Settings</strong>.
</li>
<li style="margin-bottom:4px;">
  From the FreePBX left-hand side menu (under &#8220;Inbound Call Control&#8221;), select <strong>CallerID Lookup Sources</strong>
</li>
<li style="margin-bottom:4px;">
  In the section entitled &#8220;Add Source&#8221;, enter the <em>Source Description</em> (CiviCRM), and <em>Source Type</em> (MySQL). Using <em>Cache Results</em> will cache the CID lookup in AstDB. Depending on your own set up, you may or may not feel the need to enable this setting.
</li>
<li style="margin-bottom:4px;">
  In the section entitled &#8220;MySQL&#8221; (appears after you designate the <em>Source Type</em>), enter the <em>Host</em>, <em>Database</em>, <em>Username</em> and <em>Password</em> to access your CiviCRM database.
</li>
<li style="margin-bottom:4px;">
  The section entitled <em>Query</em> is where you will enter the SQL query that will return the information you want to display about on incoming calls. I used the following SQL (you can modify this as you like to alter your display information &#8211; note, the special token &#8216;[NUMBER]&#8217; is replaced with the caller ID of the incoming call):
</li>
> `<br />
SELECT CONCAT('CiviCRM: ',display_name) FROM civicrm_contact WHERE id = (SELECT id FROM civicrm_phone WHERE REPLACE(phone, '-', '') = SUBSTRING('[NUMBER]',-10))<br />
` 

<li style="margin-bottom:4px;">
  Click <strong>Submit Changes</strong>.
</li>
<li style="margin-bottom:4px;">
  Navigate to <strong>Inbound Routes</strong> (also under the &#8220;Inbound Call Control&#8221; section of the FreePBX menu).
</li>
<li style="margin-bottom:4px;">
  Select the route that you would like to use CiviCRM as the data source for.
</li>
<li style="margin-bottom:4px;">
  In the section entitled &#8220;CID Lookup Source&#8221;, select <strong>CiviCRM</strong> from the drop down menu.
</li>
<li style="margin-bottom:4px;">
  Click <strong>Submit</strong>.
</li>
  1. Scroll to the top of the page and click on the orange bar that says _Apply Configuration Changes_ and reload the settings.

Thats it!

Now on an inbound call, the name of the CiviCRM contributor in the format **CiviCRM: John Doe** is displayed in my SIP client whenever a successful lookup occurs. There are all sorts of options that can now be used to display the caller information to non-profit staff, send a [screen pop](http://www.voiceingov.org/blog/?p=326), route the call to a specific destination, etc.

CiviCRM and trixbox are a potent combination. I hope this brief explanation of how to use the two together gets more non-profits excited about using them.