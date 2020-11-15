---
id: 102
title: How to steal an election
date: 2006-10-26T13:05:23+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=102
permalink: /how-to-steal-an-election/
categories:
  - Phone Voting
---
I found an [interesting article](http://arstechnica.com/articles/culture/evoting.ars/1) today on [/.](http://www.slashdot.org) that lays out in some detail the problems with "state of the art" direct recording electronic (DRE) voting machines. There is a lot of good information in this article for those of us interested in using technology to support the elections process **AND** make elections more secure.

Some of the information on the Diebold AccuVote TS (a popular DRE voting unit) just floored me"¦

> The GEMS database stores all of the votes collected from precinct accumulators, and it&#8217;s used to do the vote tabulation for a county. Because it&#8217;s so sensitive, you might think it would be tightly secured. But you&#8217;d be wrong.
> 
> The GEMS database is a vanilla, unencrypted Microsoft Access database that anyone with a copy of Access can edit. So if you have physical access to the GEMS server&#8217;s filesystem (either locally or remotely), then it&#8217;s not too hard to just go in and have your way with the vote totals. If Access isn&#8217;t installed on a particular GEMS server, just install it from a CD-ROM, or connect remotely from a laptop and edit the database that way.

Access?!?! No wonder people are so paranoid about DRE voting, and insistent on a verifiable paper trail. I've thought a lot about security in the telephone-based voting project I am working on, and I hope to use some of the points made in this article as context to describe why I think my system will be much more secure. (Particularly since I&#8217;ll be using a real database on the backend.)

I don't want to get too far ahead of myself "“ I've still got to finish the $#@^% thing "“ but one of the things I have spent the most time on so far has been security related features.

More to come "“ stay tuned.