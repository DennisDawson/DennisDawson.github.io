---
layout: default
title: RecordService
---

<div id="home">
  <h1>Blog Posts</h1>
  <ul class="posts">
    {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
</div>
<div>
  <h1>More Stuff</h1>
  <p>And so I dreamed a dream of dreams and dreamt I dreamt again.</p>
</div>