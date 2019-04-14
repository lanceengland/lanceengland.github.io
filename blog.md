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

<ul class="posts">
{%- for post in site.posts.select {|p| p.tags contains "test"}  -%}

<li><a href="{{ post.id }}">{{ post.title }}</a></li>

{%- endfor -%}
</ul>