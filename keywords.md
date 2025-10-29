---
layout: default
title: Keyword index
nav_order: 999
---

<h1>Keyword index</h1>

{% assign all = site.pages | concat: site.docs | concat: site.collections | uniq %}

{%- comment -%}
Collect a flat list of all keywords from all pages
{%- endcomment -%}
{% assign keys = "" | split: "" %}
{% for p in all %}
  {% if p.keywords %}
    {% assign keys = keys | concat: p.keywords %}
  {% endif %}
{% endfor %}
{% assign keys = keys | uniq | sort %}

{%- for k in keys -%}
### {{ k }}
<ul>
  {%- for p in all -%}
    {%- if p.keywords and p.keywords contains k -%}
      <li><a href="{{ p.url | relative_url }}">{{ p.title }}</a></li>
    {%- endif -%}
  {%- endfor -%}
</ul>
{%- endfor -%}
