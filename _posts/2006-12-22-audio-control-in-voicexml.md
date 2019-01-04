---
id: 109
title: Audio Control in VoiceXML
date: 2006-12-22T17:32:05+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=109
permalink: /audio-control-in-voicexml/
original_post_id:
  - "109"
categories:
  - Development Tools
  - General Discussion
---
One of the questions I hear most from VoiceXML developers relates to audio control features in VoiceXML. VoiceXML does not natively support the ability to &#8220;rewind&#8221; and &#8220;fast forward&#8221; through audio files, but some vendors provide this functionality as extensions on their platforms.

While this is nice, I like to keep my code as portable as possible. Given that VoiceXML platforms are <a href="http://www.w3.org/TR/voicexml20/#dmlAAudioFormats" target="_blank">required to support the WAV audio format</a> and this format is generally well understood, it should be relatively easy to construct server-side logic to simulate &#8220;rewind&#8221; and &#8220;fast forward&#8221; functionality.

For this example, I use PHP but I&#8217;m confident that it could be replicated in many other languages. There are several key ingredients to this example:

  * An understanding of the <a href="http://ccrma.stanford.edu/courses/422/projects/WaveFormat/" target="_blank">layout of a WAV file</a>. There are two portions that we are most interested in &#8211; the **Subchunk2Size** (which contains information on the size of the audio file) and audio data itself.
  * A platform that supports the VoiceXML 2.1 specification. If we&#8217;re going to implement these features, we need to be able to determine where in the playback of an audio file a caller issues a command. The <mark> element does this nicely, and it is supported on the <a href="http://www.voxeo.com/prophecy/" target="_blank">Voxeo Prophecy platform</a>.

The approach is simple. Using the <mark> element, and a preset interval for moving backwards and forwards (I use a setting of 5 seconds in this example) we &#8220;rebuild&#8221; the audio file each time a user issues a command. A PHP script takes in a parameter for the offset and reads in the source file from that point, and then outputs the contents of the audio file from that point.

For example, if a caller listens to an audio file for 10 seconds and then issues a command to &#8220;fast forward&#8221; we need to have our audio file begin playing at a point 15 seconds from the beginning of the file (10 + 5 = 15).

If the caller then listens to the file for 10 more seconds, and issues a command to rewind, we need to play the file starting at an offset of 20 seconds (15 + 10 = 25; 25 &#8211; 5 = 20). Using the PHP &#8220;<a href="http://us3.php.net/manual/en/function.fread.php" target="_blank">fread</a>&#8221; and &#8220;<a href="http://us3.php.net/manual/en/function.unpack.php" target="_blank">unpack</a>&#8221; functions, we can read in the contents of an audio file on the fly, based on how long the caller has listened to the file and the command they have issued.

A pair of very simple scripts can be [downloaded here](http://www.voiceingov.org/xfiles/audio_control.zip). Please bear in mind that these are still very rough, but they do demonstrate the basic idea.

Any comments or suggestions are welcome.

Note &#8212; this example is built on ideas and code originally obtained from <a href="http://snipplr.com" target="_blank">Snippler</a>. The example that I used can be found <a href="http://snipplr.com/view/285/read-wav-header-and-calculate-duration/" target="_blank">here</a>.