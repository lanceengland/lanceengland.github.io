# Blog Index

This page will list all entries and maybe categories.

{% for post in site.posts %}
- [{{ post.title }}]({{ post.id }})
{% endfor %}