---
layout: default
title: My articles by Ismail Mechbal
---

<div class="home" id="home">
  <h2 class="pageTitle">Articles</h2>
  <ul class="posts noList">
    {% for post in site.posts %}
      <li>
      	<span class="date">{{ post.date | date_to_string }}</span>
      	<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      	<p class="description">{% if post.description %}{{ post.description  | strip_html | strip_newlines | truncate: 120 }}{% else %}{{ post.content | strip_html | strip_newlines | truncate: 120 }}{% endif %}</p>
      </li>
    {% endfor %}
  </ul>
</div>