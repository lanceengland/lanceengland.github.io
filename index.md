# Placeholder

A [post](/2019/04/blog-move) on moving my blog.

## Posts (new code v2)

{% for post in site.posts %}

- [{{ post.title }}]({{ post.id }})

{% endfor %}

List of images:

{% assign image_files = site.static_files | where: "image", true %}
{% for myimage in image_files %}
  {{ myimage.path }}
{% endfor %}
