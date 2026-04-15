---
layout: default
title: "Tags"
permalink: /tags/
---

<div class="home">
  <h1 class="page-heading">Tags</h1>

  <div class="tag-cloud">
    {% assign sorted_tags = site.tags | sort %}
    {% for tag in sorted_tags %}
      <a class="tag-link" href="#{{ tag[0] | slugify }}">{{ tag[0] }} ({{ tag[1].size }})</a>
    {% endfor %}
  </div>

  {% assign sorted_tags = site.tags | sort %}
  {% for tag in sorted_tags %}
    <h2 id="{{ tag[0] | slugify }}">{{ tag[0] }}</h2>
    <ul class="post-archives">
      {% for post in tag[1] %}
        <li>
          <span class="post-meta">{{ post.date | date: "%Y-%m-%d" }}</span>
          <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
        </li>
      {% endfor %}
    </ul>
  {% endfor %}
</div>
