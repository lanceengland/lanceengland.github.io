# Blog Index

## Archive

<ul class="posts">
{%- for post in site.posts -%}
{%- unless post.tags contains "test" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endunless -%}
{%- endfor -%}
</ul>

## Test

<ul class="posts">
{%- for post in site.posts  -%}
{%- if post.tags contains "test" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>