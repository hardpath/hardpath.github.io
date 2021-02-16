---
title: Posts: Operational Technology
---

{% for post in site.posts %}
{% if post.categories contains "ot" %}
[{{ post.title }}]({{ post.url }})
{% endif %}
{% endfor %}