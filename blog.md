# Blog Index

## Archive
<ul>
{% for post in site.posts %}
{% unless post.tags contains "test" %}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{% endunless %}
{% endfor %}
</ul>

## Test

{% for post in site.posts  %}
{% if post.tags contains "test" %}
- [{{ post.title }}]({{ post.id }})
{% endif %}
{% endfor %}