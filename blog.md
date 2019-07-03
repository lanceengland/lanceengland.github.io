---
date : 2019-04-14 07:27:08
---
# Blog

All blog posts by category.

<!--- Data --->
{%- assign has_data_posts = false -%}
{%- for post in site.posts -%}
    {%- if post.tags contains "data" -%}
        {%- assign has_data_posts = true -%}
        {% break %}
    {%- endif -%}
{%- endfor -%}

{% if has_data_posts %}
<a name="data"></a><h2>Data</h2>
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
<a name="analysis"></a><h2>Analysis</h2>
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
{% raw %}<a name="integration"></a><h2>Integration</h2>{% endraw %}
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
{% raw %}<a name="automation"></a><h2>Automation</h2>{% endraw %}
<ul class="posts">
{%- for post in site.posts -%}
{%- if post.tags contains "automation" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endif -%}
{%- endfor -%}
</ul>
{%- endif -%}

<!--- Other --->
{%- assign has_other_posts = false -%}
{%- for post in site.posts -%}
    {%- unless post.tags contains "data" or post.tags contains "analysis" or post.tags contains "integration" or post.tags contains "automation" -%}
        {%- assign has_other_posts = true -%}
        {% break %}
    {%- endunless -%}
{%- endfor -%}

{% if has_other_posts %}
{% raw %}<h2>Other</h2>{% endraw %}
<ul class="posts">
{%- for post in site.posts -%}
{%- unless post.tags contains "data" or post.tags contains "analysis" or post.tags contains "integration" or post.tags contains "automation" -%}
<li><a href="{{ post.id }}">{{ post.title }}</a></li>
{%- endunless -%}
{%- endfor -%}
</ul>
{%- endif -%}
