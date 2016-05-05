---
layout: archive
author_profile: true
permalink: /posts.html
---
{% include base_path %}
<h3 class="archive__subtitle">All Posts</h3>
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>