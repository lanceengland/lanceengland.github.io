# Placeholder

A [post](/2019/04/blog-move) on moving my blog.

## Posts (new code v2)

{% for post in site.posts %}

- {{ post.date }} [{{ post.title }}]({{ post.id }})

{% endfor %}
