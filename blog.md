---
date : 2019-04-14 07:27:08
---
# Blog Index

I will get this working!

## Data

## Analysis

## Integration

## Other

<ul class="posts">
{%- for post in site.posts -%}
{%- unless post.tags contains "test" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endunless -%}
{%- endfor -%}
</ul>

{%- assign test_posts = site.posts | where: "tags","test" -%}
{% if test_posts %}
test collection found
{% else %}
test collection NOT found
{% endif %}

## Test

<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "test" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
