---
layout: default
title: Posts
permalink: /posts
---

<div id="home">
  <h1>Blog Posts</h1>
  <ul class="posts">
    {% for post in site.posts %}
      {% if post.tags contains 'draft' %}
      {% else %}
        <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}

    {% endfor %}
  </ul>
</div>
