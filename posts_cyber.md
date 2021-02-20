<table>
    {% for post in site.posts %}
    {% if post.categories contains "cyber" %}
        {% include post_entry.html %}
    {% endif %}
    {% endfor %}
</table>

