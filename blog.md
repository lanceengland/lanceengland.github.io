---
date : 2019-04-14 07:27:08
---
# Blog Index

Below is an index of all blog posts arranged by topic.

{%- assign has_data_posts = false -%}
{%- for post in site.posts -%}
    {%- if post.tags contains "data" -%}
        {%- assign has_data_posts = true -%}
        {% break %}
    {%- endif -%}
{%- endfor -%}

{% if has_data_posts %}
{% raw %}<h1>Data</h1>{% endraw %}
<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "data" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}
