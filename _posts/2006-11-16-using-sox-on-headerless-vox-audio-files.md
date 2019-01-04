---
id: 107
title: Using SoX on Headerless (VOX) Audio files
date: 2006-11-16T16:35:31+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=107
permalink: /using-sox-on-headerless-vox-audio-files/
categories:
  - Development Tools
  - General Discussion
---
Recently, I had occasion to use <a href="http://sox.sourceforge.net/" target="_blank">Sound eXchange</a> (SoX), the open source sound processing program that lets you convert audio files into different formats, add sound effects to audio files, mix multiple audio files together, and a bunch of other cool stuff.

I really like this tool &#8212; it&#8217;s powerful, easy to use and easy to incorporate into scripts for batch processing. I did, however, run into a bit of a snag when using it to try and convert audio files in &#8220;.vox&#8221; format (used on one VoiceXML platform) to &#8220;.wav&#8221; format (for use on another platform). I simply could not get SoX to convert my &#8220;.vox&#8221; files to &#8220;.wav&#8221; files successfully. Every &#8220;.wav&#8221; audio file I generated was garbled nonsense that sounded only vaguely like human speech.

Trying things like&#8230;

<pre>~$ sox myfile.vox myfile.wav
</pre>

or

<pre>~$ sox -r 8000 -t vox -c 1 myfile.vox myfile.wav
</pre>

&#8230;failed miserably. I had a hard time figuring out why. I could find almost nothing helpful on the web.

According to the SoX documentation:

> SoX attempts to determine the file type of input files automatically by looking at the header of the audio file. When it is unable to detect the file type or if its an output file then it uses the file extension of the file to determine what type of file format handler to use.

The thing about &#8220;.vox&#8221; files is that <a href="http://en.wikipedia.org/wiki/VOX_%28file_format%29" target="_blank">they are headerless</a> &#8212; they don&#8217;t have descriptive information that can be read in by utilities that play these files. The utility has to be told things like sample rate, encoding format, etc. But based on my read of the documentation, I thought that SoX would pick this up through the &#8220;.vox&#8221; extension on the file.

After a number of unsuccessful attempts, I decided to try again taking a bit of a different approach. The SoX documentation also states that &#8220;audio data can usually be totally described by four characteristics&#8221;

  * The sample rate;
  * The precision the data is stored in;
  * The data encoding, and;
  * The number of channels

When I told SoX to treat the input file as a raw file (i.e., ignore the &#8220;.vox&#8221; extension), and then specifically told SoX each of the four characteristics of my file, it finally worked.

<pre>~$ sox -t raw -r 8000 -c 1 -U -b myfile.vox myfile.wav
</pre>

  * The -t option tells SoX that the input file is raw (headerless) audio
  * The -r option tells SoX the sample rate of the input file
  * The -c option tells SoX that the input file has one channel (mono)
  * The -U option tells SoX that the data encoding is u-law
  * The -b option tells SoX that the data size is in bytes

My generated &#8220;.wav&#8221; files are clear as a bell and sound great. In the end, it turned out not to be a SoX issue as much as a lack of information on how to use it properly with &#8220;.vox&#8221; files, which is a rather old format but is still used on many telephony platforms. I hope this post helps someone else down the line struggling with the same issue.