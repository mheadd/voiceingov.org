---
id: 128
title: 'What&#8217;s MIME is Not Yours'
date: 2008-01-01T12:50:11+00:00
author: mheadd
layout: post
guid: http://www.voiceingov.org/blog/?p=128
permalink: /whats-mime-is-not-yours/
jd_tweet_this:
  - 'yes'
original_post_id:
  - "128"
categories:
  - Tutorials
---
Continuing on the theme of safety and security in VoiceXML applications, I&#8217;ve been thinking a lot lately about MIME types and how VoiceXML browsers request content from a web server.

When any browser (VoiceXML or otherwise) requests a document from a server, it sends along information in the request telling the server what media types it can consume. This information is usually conveyed in the HTTP accept header as a MIME type. A VoiceXML browser should explicitly request documents from a web server using the VoiceXML MIME type (â€œapplication/voicexml+xmlâ€) and servers should use this MIME type when responding to such requests (instead of sending back plain old â€œtext/xmlâ€).

If a browser can&#8217;t consume VoiceXML, what&#8217;s the point of responding with it (unless were doing something legitimate, like debugging). It was with this thought in mind that I decided to throw together a quick script to inspect the Accept header on HTTP requests and to only send back VoiceXML content if the appropriate MIME type is included in the request.

### Classes for Inspecting HTTP Headers

This example will use two small PHP classes to inspect HTTP headers. The parent class looks like this:

<pre>// A simple parent class to read HTTP request headers

class readHeaders {

Â Â Â  private $httpHeaderArray;

Â Â Â  function __construct(Array $headerArray) {
Â Â Â Â Â  $this-&gt;httpHeaderArray = $headerArray;
Â Â Â  }

Â Â Â  function getHeaderCount() {
Â Â Â Â Â  return count($this-&gt;httpHeaderArray);
Â Â Â  }

Â Â Â  function getHeaderValue($name) {
Â Â Â Â Â  try {
Â Â Â Â Â Â Â  if(isset($this-&gt;httpHeaderArray[$name])) {
Â Â Â Â Â Â Â Â Â  return $this-&gt;httpHeaderArray[$name];
Â Â Â Â Â Â Â  } else {
Â Â Â Â Â Â Â Â Â  return 'Sorry.Â  Could not find that header.';
Â Â Â Â Â Â Â  }
Â Â Â Â Â  }

Â Â Â Â Â  catch (Exception $e) {
Â Â Â Â Â Â Â  return 'There was a problem: '.$e-&gt;getMessage();
Â Â Â Â Â  }
Â Â Â  }
Â  }

</pre>

The class is instantiated with an array containing all of the HTTP headers that are sent with the request. There are two methods; one to determine the number of headers values and the other to retrieve a specific header value.

The child class for our example looks like this:

<pre>// A simple child class to inspect the HTTP Accept header
  class parseAcceptHeader extends readHeaders {

    function __construct(Array $headerArray) {

      parent::__construct($headerArray);
    }

    function parseHeader($pattern) {

      $acceptHeaderValue = parent::getHeaderValue('accept');

      if(@strstr($acceptHeaderValue, $pattern)) {
        return true;
      } else {
        return false;
      }

    }
  }

</pre>

This class uses a specific pattern (passed in to its one an only method as a string) and looks for it in the accept header. We return a boolean value depending on whether the string is found in the header value (note â€“ many browsers, including VoiceXML browsers, will use a comma delimited string of different MIME types in the accept header)

### Using the HTTP Header Inspection Classes

Now that we have our two simple classes, we use them like so:

<pre>// Get HTTP Request headers
  $headers = apache_request_headers();

  // Instantiate new object to inspect Accept header
  $myValue = new parseAcceptHeader($headers);
  $myPattern = 'application/voicexml+xml';

  /*
   If the requesting browser doesn't explicitly accept VoiceXML
   give it a 404 error
 */
  if(!$myValue-&gt;parseHeader($myPattern)) {

    header('HTTP/1.0 404 Not Found');
    exit();

  }

  // Otherwise, send the appropriate MIME type back in the response
  header('Content-type: $myPattern');

  // Write out the the VoiceXML content to be returned
  ...
</pre>

Of particular note here is our use of the PHP [apache\_request\_headers()](http://us2.php.net/manual/en/function.apache-request-headers.php) function. This function will return an associative array of all the HTTP headers and their values. Use of this function is limited, however, to instances where PHP is running as an Apache module.

In this example, we check specifically for the presence of the â€œapplication/voicexml+xmlâ€ content type in the HTTP headers. If we find it, we send back VoiceXML; if not, we simply respond with a 404 error and stop execution of the script.

This simple example is just one way of ensuring that your VoiceXML code is obscured form prying eyes. Its far from perfect â€“ anyone with some chops can easily construct an HTTP request with the appropriate MIME type and look at your VoiceXML code in an effort to footprint your application for possible SQL injection.

However, an approach like this one (and those discussed in the previous post) can make life harder for the average web surfer intent on doing a little mischief. It might also make it harder for automated vulnerability scanners to footprint your applications.

Its a new year. Be safe.