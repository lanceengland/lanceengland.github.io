---
date : 2019-04-14 07:27:08
---
# Blog

Below is an index of all blog posts arranged by topic.

<!--- Data --->
{%- assign has_data_posts = false -%}
{%- for post in site.posts -%}
    {%- if post.tags contains "data" -%}
        {%- assign has_data_posts = true -%}
        {% break %}
    {%- endif -%}
{%- endfor -%}

{% if has_data_posts %}
{% raw %}<h2>Data</h2>{% endraw %}
<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "data" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}

<!--- Analysis --->
{%- assign has_analysis_posts = false -%}
{%- for post in site.posts -%}
    {%- if post.tags contains "analysis" -%}
        {%- assign has_analysis_posts = true -%}
        {% break %}
    {%- endif -%}
{%- endfor -%}

{% if has_analysis_posts %}
{% raw %}<h2>analysis</h2>{% endraw %}
<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "analysis" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}

<!--- Integration --->
{%- assign has_integration_posts = false -%}
{%- for post in site.posts -%}
    {%- if post.tags contains "integration" -%}
        {%- assign has_integration_posts = true -%}
        {% break %}
    {%- endif -%}
{%- endfor -%}

{% if has_integration_posts %}
{% raw %}<h2>Integration</h2>{% endraw %}
<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "integration" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}

<!--- Automation --->
{%- assign has_automation_posts = false -%}
{%- for post in site.posts -%}
    {%- if post.tags contains "automation" -%}
        {%- assign has_automation_posts = true -%}
        {% break %}
    {%- endif -%}
{%- endfor -%}

{% if has_automation_posts %}
{% raw %}<h2>automation</h2>{% endraw %}
<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "automation" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}
