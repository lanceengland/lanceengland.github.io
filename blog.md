---
date : 2019-04-14 07:27:08
---
# Blog Index

Below is an index of all blog posts arranged by topic.

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

{%- assign analysis_posts = site.posts | where: "tags","analysis" -%}
{%- if analysis_posts -%}

## Data

<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "analysis" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}

{%- assign integration_posts = site.posts | where: "tags","integration" -%}
{%- if integration_posts -%}

## Integration

<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "integration" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}

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
