# Blog Index

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

## Testing select array function

{%- assign test_posts = site.posts | where: "tags","test" -%}
<ul class="posts">
{%- for post in test_posts -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endfor -%}
</ul>
