---
date : 2019-04-14 07:27:08
---
# Blog Index

{%- assign data_posts = site.posts | where: "tags","data" -%}
{%- if data_posts -%}

## Data

<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "data" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}

## Analysis

todo

## Integration

todo

## Other

<ul class="posts">
{%- for post in site.posts -%}
{%- unless post.tags contains "test" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endunless -%}
{%- endfor -%}
</ul>

{% comment %} clunky collection filtering {% endcomment %}
{%- assign test_posts = site.posts | where: "tags","test" -%}
{%- if test_posts -%}

## Test (about to delete if this works)

<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "test" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}
