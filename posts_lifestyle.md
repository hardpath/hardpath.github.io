<table>
    {% for post in site.posts %}
        {% if post.categories contains "lifestyle" %}
            {% include post_entry.html %}
        {% endif %}
    {% endfor %}
</table>

