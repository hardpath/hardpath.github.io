---
title: Posts: Operational Technology
---

{% for post in site.posts %}
{% if post.categories contains "ls" %}
[{{ post.title }}]({{ post.url }})
{% endif %}
{% endfor %}