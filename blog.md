---
layout: default
title: Blog
permalink: /blog/
---

## All Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> – 
      <small>{{ post.date | date_to_string }}</small>
    </li>
  {% endfor %}
</ul>