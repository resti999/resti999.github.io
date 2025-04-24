---
layout: default
title: Tags
permalink: /tags/
---

<h1>Tags</h1>
<ul>
  {% assign tags = site.tags | sort %}
  {% for tag in tags %}
    <li><a href="{{ '/tags/' | append: tag[0] | relative_url }}">{{ tag[0] }}</a> ({{ tag[1].size }})</li>
  {% endfor %}
</ul>