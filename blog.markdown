---
layout: page
title: Blog
permalink: /blog/
---

#### Welcome!
I will be sharing some of my thoughts on papers I read in the fields of Offensive Security and AI. See the list of papers I’ve read so far below.

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
