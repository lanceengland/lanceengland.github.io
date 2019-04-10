# Placeholder

A [post](/2019/04/blog-move) on moving my blog.

## Posts (new code v2)

{% for post in site.posts %}

- [{{ post.title }}]({{ post.id }})

{% endfor %}

Image

![T-SQL Tuesday](/assets/img/TSQL2sDay150x150.jpg)

Image as link

[![T-SQL Tuesday](/assets/img/TSQL2sDay150x150.jpg)](http://www.bobpusateri.com/archive/2013/04/invitation-to-t-sql-tuesday-41-presenting-and-loving-it/)

List of images:

{% assign image_files = site.static_files | where: "image", true %}
{% for myimage in image_files %}
  {{ myimage.path }}
{% endfor %}

Checklist test

- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item