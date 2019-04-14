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

## Testing filtered array

{%- assign test_posts = site.posts | where: "tags","test" -%}

<ul class="posts">
{%- for post in test_posts -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endfor -%}
</ul>
