---
id: 117
title: 'Top 5 Things to do after intalling Trixbox: #2'
date: 2007-05-16T11:31:11+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=117
permalink: /top-5-things-to-do-after-intalling-trixbox-2/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "117"
categories:
  - General Discussion
  - trixbox
  - VoIP
tags:
  - Asterisk
---
Once you&#8217;ve got your Trixbox all set up, you need to think about backups "“ what happens if your system needs to be restored, or if you&#8217;re doing and upgrade (say from version 2.0 to 2.2)?

The FreePBX module in Trixbox has a handy "System Backup" feature "“ in FreePBX, go to Tools->Backup & Restore.

Click on the "Add Backup Schedule" link and name your new schedule "“ I personally take the simple approach of just doing a weekly backup on Sunday night.

When your backup is created, Trixbox places a ".tar.gz" file in the /var/lib/asterisk/backups/ directory. So, if you created a schedule called "Weekly" your backup file would be placed in the directory /var/lib/asterisk/backups/Weekly/. Here these files will stay "“ a new one is created each time your scheduled backup is run "“ unless you do something with them.

For obvious reasons, we want to move these backup files to another server for safe keeping. Additionally, one we&#8217;ve successfully moved our backup, we want to clean up the old files from our backup directory.

There are <a href="http://www.sureteq.com/asterisk/trixboxv2.2.htm#18.1_-_Backing_up_Trixbox" target="_blank">any number</a> of ways of doing this. Here is my preferred method:

> Create a new user on your Trixbox machine, and add that user to the "asterisk" group. (The user will need to be a part of this group to access the backups directory.)
> 
> **useradd -G asterisk backup
  
> passwd backup**
> 
> Log in as this new user, and create a key pair for ssh connections. When prompted to enter a pass phrase, simply hit enter (blank pass phrase).
> 
> **ssh-keygen -t dsa**
> 
> Export your public key to the machine where you will be storing your backups.
> 
> **scp ~/.ssh/id\_dsa.pub username@xxx.xxx.xxx.xxx:.ssh/authorized\_keys**
> 
> Make sure you can SSH to the machine where you will store your back up files. You&#8217;ll want to make sure that key-based authentication is working (i.e., that your not prompted for a password). 
> 
> **ssh username@xxx.xxx.xxx.xxx**
> 
> Make sure you can securely copy files to the machine where you will store your back up files (again, make sure that your not prompted for a password).
> 
> **scp /var/lib/asterisk/backups/Weekly/* username@xxx.xxx.xxx.xxx:trixbox_backups/**

If all of the above steps went successfully, you are now ready to set up your automated process. We&#8217;ll use CRON to do this.

> Log on as the new backup user on your Trixbox machine.
> 
> Create a new crontab for this user.
> 
> **crontab -e
  
>** 
> 
> Insert cron entries to copy backup files to the remote machine, and to clean up old files after they have been moved.
> 
> **01 0 \* \* 1 scp /var/lib/asterisk/backups/Weekly/* username@xxx.xxx.xxx.xxx:trixbox_backups/
  
> 
  
> 01 1 \* \* 1 rm -f /var/lib/asterisk/backups/Weekly/***
> 
> The first entry will move our weekly backup file at 12:01 a.m. on Monday morning to our remote server. The second entry will clear the Weekly directory an hour later. This way, when you go to the office on Monday, all of your backup process are done and your ready to start the week on a good note.

In the next article, we&#8217;ll discuss how to monitor your Trixbox to make sure that it is performing properly.