<table>
    {% for post in site.posts %}
        {% if post.categories contains "ot" %}
            {% include post_entry.html %}
        {% endif %}
    {% endfor %}
</table>

