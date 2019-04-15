---
date : 2019-04-14 07:27:08
---
# Blog Index

Below is an index of all blog posts arranged by topic.

{%- assign data_posts = site.posts | where: "tags","data" -%}
{%- if data_posts -%}
<h1>Data</h1>
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
<h1>Analysis</h1>
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
<h1>Integration</h1>
<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "integration" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}

{%- assign automation_posts = site.posts | where: "tags","automation" -%}
{%- if automation_posts -%}
<h1>automation</h1>
<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "automation" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}
