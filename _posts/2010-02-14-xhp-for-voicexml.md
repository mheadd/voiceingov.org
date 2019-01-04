---
id: 1607
title: XHP for VoiceXML
date: 2010-02-14T18:23:08+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=1607
permalink: /xhp-for-voicexml/
jd_tweet_this:
  - 'yes'
jd_twitter:
  - "[New Blog Post] Using Facebook's XHP extension to #PHP for #VoiceXML. #url# #opensource #facebook #ubuntu"
wp_jd_target:
  - http://www.voiceingov.org/blog/?p=1607
wp_jd_clig:
  - http://cli.gs/VMgJD
original_post_id:
  - "1607"
categories:
  - Development Tools
  - Open Source
---
Facebook has been busy lately doing all sorts of interesting things to the PHP scripting language. Although most of the recent PR hype was centered around [HipHop for PHP](http://wiki.github.com/facebook/hiphop-php/) (a name that, quite frankly, makes me want to use it less), Facebook also released another very interesting and potentially useful extension for PHP &#8211; [XHP](http://github.com/facebook/xhp). From the [XHP wiki on Github](http://wiki.github.com/facebook/xhp/):

> [XHP](http://github.com/facebook/xhp) is a PHP extension which augments the syntax of the language such that XML document fragments become valid PHP expressions. This allows you to use PHP as a stricter templating engine and offers much more straightforward implementation of reusable components. 

Not sure where I read it, but one commenter compared XHP to [E4X in JavaScript](https://developer.mozilla.org/En/E4X/Processing_XML_with_E4X). It&#8217;s a neat idea, and its actually not all that hard to start playing around with XHP (if you&#8217;re comfortable installing PHP extensions).

After thinking about XHP and playing around with it a bit, I started to think it might be useful as a way of generating not just HTML, but also other XML-based languages like VoiceXML.

The core XHP classes that are used for generating HTML are fairly easy to understand, once you get use to the syntax &#8211; extending these core classes to generate VoiceXML (or any other XML-based language) is not all that hard. But before we do that, let&#8217;s install XHP as a PHP extension and kick the tires a bit.

### Installing XHP on Ubuntu

I&#8217;ve tried the following instructions on both Ubuntu 8.04 and 8.10, and I&#8217;m pretty sure they&#8217;ll work with just about any recent Ubuntu version.

Make sure you&#8217;ve got the basics for building PHP extensions:

> $ sudo apt-get install build-essential
  
> $ sudo apt-get install php5-dev 

Get the XHP source code:

> $ mkdir xhp
  
> $ cd xhp
  
> $ git clone git://github.com/facebook/xhp.git 

Get the PHP source code

> $ sudo apt-get source php5 

Make a directory in the PHP source code for the XHP extension

> $ mkdir php-5.X.X/ext/xhp
  
> $ cp -r xhp/xhp/* php-5.X.X/ext/xhp/ 

Depending on your system, you may need to add some of the prerequisites for building this extension:

> $ sudo apt-get install flex bison 

I tried this approach on both Ubuntu 8.04 and 8.10 and in both cases only version 0.13.5 of re2c worked (earlier version obtained via apt-get did not cut the mustard). If you&#8217;re running a later version, you may be able to get this version of re2c from the standard repos.

For this example, I&#8217;ll just build it from source:

> $ wget http://sourceforge.net/projects/re2c/files/re2c/0.13.5/re2c-0.13.5.tar.gz/download
  
> $ tar -zxvf re2c-0.13.5.tar.gz
  
> $ cd re2c-0.13.5/
  
> $ ./configure
  
> $ sudo make
  
> $ sudo make install 

Move to new XHP extension directory and get &#8216;er done.

> $ cd php-5.X.X/ext/xhp
  
> $ phpize
  
> $ ./configure
  
> $ sudo make
  
> $ sudo make install 

When you run `make install`, the new extension file is placed in a directory where Apache can load it &#8211; however, we need to modify the `php.ini` file so that Apache is aware of the extension. Open `php.ini` and add the following:

> extension=xhp.so
  
> extension\_dir=directory\_where\_xhp.so\_is_located 

When setting this last option, use the directory where xhp.so was placed by `make install`. Now we just restart Apache:

> $ sudo /etc/init.d/apache2 restart 

Easy right? Unfortunately, things get a little less clear at this point. It turns out that to make XHP work properly, some PHP libraries need to be included in any of the XHP scripts we write. These files are located in the `php-lib` directory of the XHP source code.

To make things easier (especially when we start to write our own extension to XHP for VoiceXML, lets move these files to a convenient local directory that we can include in our scripts:

> $ cp php-5.X.X/ext/xhp/php-lib/* my/local/directory/php-lib/ 

So now we can do some interesting stuff like this:

What&#8217;s especially interesting about XHP is that it enforces proper syntax at compile time, so if your markup isn&#8217;t syntactically correct an exception gets tossed.

### Generating VoiceXML with XHP

The XHP libraries we just discussed implement the HTML spec out of the box. However, if you try and render tags that are not part of the HTML spec an exception will occur. I wanted to find out how hard it would be to extend the concepts behind XHP for other markup languages, like VoiceXML. Turns out, its not hard at all.

If you look at the `init.php` file that is included in the example above, you&#8217;ll see its in turn including a file called html.php, which defines all of the HTML elements that can be rendered by XHP, the attributes that each can have and also the parent-child relationship between elements.

Using this is a guide (the syntax is new but fairly easy to follow), I knocked out a quick class file for some basic VoiceXML elements &#8211; just to illustrate the concept:

This is admittedly rough, and it only covers a few basic VoiceXML elements, but it demonstrates that extending XHP to render VoiceXML is actually quite easy. To use this file, simple edit the `init.php` file mentioned previously and add an include statement:

> 
Now we&#8217;re ready to use XHP to generate VoiceXML:

How cool is that?

I&#8217;m still toying around with XHP, but this little experiment clearly shows that it has use beyond just simply rendering HTML. I&#8217;d be interested in hearing from other developers &#8211; is this worth a full blown project to flush out a complete VoiceXML class library for XHP? What other markup languages would make good candidates for this same type of approach?

Post a comment here, a tweet to [@mheadd](http://twitter.com/mheadd) or shoot a quick e-mail to mheadd [at] voiceingov.org with your thoughts or comments.