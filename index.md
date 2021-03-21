<table style="margin-top: 40px;">
    {% for post in site.posts %}
        {% unless post.categories contains "portfolio" %}
            {% include post_entry.html %}
        {% endunless %}
    {% endfor %}
</table>

