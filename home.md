---
layout: base
permalink: /
---

## The Voice(XML) of the People

This is a copy of a website that I maintained for many years where I shared details of phone and voice-based technologies for use by governments. This site is no longer active, and the content is retained here in the hope that it will be useful to others. The source code for this website [can be found on Github](https://github.com/mheadd/voiceingov.org).

### Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> ({{ post.date | date: '%B %d, %Y' }})
    </li>
  {% endfor %}
</ul>
