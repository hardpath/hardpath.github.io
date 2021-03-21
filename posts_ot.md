<table style="margin-top: 20px;">
    <tr>
        <td class="section">Operational Technology</td>
    </tr>
</table>
<table style="margin-top: 40px;">
    {% for post in site.posts %}
        {% if post.categories contains "ot" %}
            {% include post_entry.html %}
        {% endif %}
    {% endfor %}
</table>

