{% assign latestPost = site.posts.first %}
{{ latestPost.content }}

## Posts

{% for post in site.posts %}
[{{ post.title }}]({{ post.id }})

{% endfor %}