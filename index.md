---
title: Lance-England.com
---
{{ site.posts.first.content }}

{% if site.posts.first.id %}
<div id="post-info">
    <img style="width:1em" src="/assets/img/clock.svg" /><span>{{ site.posts.first.date | date_to_string }}</span>
    <a href="{{ site.posts.first.id }}"><img style="width:1em" src="/assets/img/link.svg" /></a><span>Permalink</span>
    <img style="width:1em" src="/assets/img/tags.svg" /> <span>{%- for tag in site.posts.first.tags -%}<a href="/blog#{{ tag }}">{{ tag }}</a>&nbsp;{%- endfor -%}</span>
</div>
{% endif %}