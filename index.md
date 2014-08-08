---
layout: default
title: Where there is a will, there is a way
---
# {{ page.title }}
## 文章列表

<ul>
    {% for post in site.posts %}
      <li>{{ post.date | date_to_string }} <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
