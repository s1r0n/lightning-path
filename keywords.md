---
# Temporarily avoid layout errors while theme is being fixed.
# After the theme is loading, change to: layout: default
layout: default
title: Keyword index
nav_order: 999
permalink: /keywords/
---

<h1>Keyword index</h1>

{%- comment -%}
Collect a single array of all documents (pages + each collection's docs)
{%- endcomment -%}
{% assign docs = site.pages %}

{% for coll in site.collections %}
  {% if coll.docs %}
    {% assign docs = docs | concat: coll.docs %}
  {% endif %}
{% endfor %}

{%- comment -%}
Build a normalized list of all keywords (accept array OR string in front matter)
{%- endcomment -%}
{% assign keys = "" | split: "" %}
{% for p in docs %}
  {% if p.keywords %}
    {% if p.keywords.first %}
      {# p.keywords is an array #}
      {% assign keys = keys | concat: p.keywords %}
    {% else %}
      {# p.keywords is a string #}
      {% assign keys = keys | push: p.keywords %}
    {% endif %}
  {% endif %}
{% endfor %}
{% assign keys = keys | uniq | sort %}

{%- if keys.size == 0 -%}
<p><em>No keywords found. Add <code>keywords: [tag1, tag2]</code> to your pages’ front matter.</em></p>
{%- endif -%}

{%- for k in keys -%}
<h2 id="kw-{{ k | slugify }}">{{ k }}</h2>
<ul>
  {%- for p in docs -%}
    {%- if p.keywords -%}
      {%- assign is_array = p.keywords.first -%}
      {%- if is_array -%}
        {%- assign joined = p.keywords | join: "," -%}
      {%- else -%}
        {%- assign joined = p.keywords -%}
      {%- endif -%}
      {%- if joined contains k -%}
        <li><a href="{{ p.url | relative_url }}">{{ p.title | default: p.name }}</a></li>
      {%- endif -%}
    {%- endif -%}
  {%- endfor -%}
</ul>
{%- endfor -%}
](https://repo.lightningpath.org/keywords/)
