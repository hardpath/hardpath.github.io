{% if page.tn_ref != "" and page.tn_ref != null %}
    {% include posts_header_tn.html %}
{% else %}
    {% include posts_header_img.html %}
{% endif %}
