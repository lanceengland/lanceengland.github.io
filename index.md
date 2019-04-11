# Placeholder

This will probably be set up to display the latest blog post content with the permalink as the title

## List with paragragh tag

- line 1
- line 2
- line 3

## Posts

{% for post in site.posts %}
- [{{ post.title }}]({{ post.id }})
{% endfor %}

List of images:

{% assign image_files = site.static_files | where: "image", true %}
{% for myimage in image_files %}
  {{ myimage.path }}
{% endfor %}

