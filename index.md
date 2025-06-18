---
layout: default
title: "Sunghak's Dev Blog"
---

# 👋 Welcome to My Blog

The place to report my dev journey

---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> ({{ post.date | date: "%Y-%m-%d" }})
    </li>
  {% endfor %}
</ul>
