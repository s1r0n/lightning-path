---
# While you're still seeing layout warnings, keep layout: null so this page renders.
# Once the theme is loaded properly, change to: layout: default
layout: null
title: Keyword index
nav_order: 999
permalink: /keywords/
---

<h1>Keyword index</h1>
<p>Docs grouped by <code>keywords</code> front matter. Use arrays:
<code>keywords: [ai, pedagogy, brain]</code> or a comma list:
<code>keywords: ai, pedagogy, brain</code>.</p>

{%- comment -%} Collect all pages + collection docs {%- endcomment -%}
{%- assign docs = site.pages -%}
{%- for coll in site.collections -%}
  {%- if coll.docs -%}
    {%- assign docs = docs | concat: coll.docs -%}
  {%- endif -%}
{%- endfor -%}

{%- comment -%} Build unique, normalized keyword list {%- endcomment -%}
{%- assign keys = "" | split: "" -%}
{%- for p in docs -%}
  {%- if p.keywords -%}
    {%- assign raw = p.keywords -%}
    {%- if raw.first -%}{% assign arr = raw %}{% else %}{% assign arr = raw | split: ',' %}{% endif -%}
    {%- for k in arr -%}
      {%- assign kclean = k | strip -%}
      {%- if kclean != "" -%}
        {%- assign keys = keys | push: kclean -%}
      {%- endif -%}
    {%- endfor -%}
  {%- endif -%}
{%- endfor -%}
{%- assign keys = keys | uniq | sort_natural -%}

{%- if keys.size == 0 -%}
<p><em>No keywords found. Add <code>keywords: [tag1, tag2]</code> to your pages’ front matter.</em></p>
{%- endif -%}

{%- comment -%} A small A–Z anchor list with per-key counts {%- endcomment -%}
<ul>
{%- for k in keys -%}
  {%- assign count = 0 -%}
  {%- for p in docs -%}
    {%- if p.url != page.url and p.keywords -%}
      {%- assign raw = p.keywords -%}
      {%- if raw.first -%}{% assign arr = raw %}{% else %}{% assign arr = raw | split: ',' %}{% endif -%}
      {%- assign hit = false -%}
      {%- for kk in arr -%}
        {%- if kk | strip == k -%}{% assign hit = true %}{% break %}{% endif -%}
      {%- endfor -%}
      {%- if hit -%}{% assign count = count | plus: 1 %}{% endif -%}
    {%- endif -%}
  {%- endfor -%}
  <li><a href="#kw-{{ k | slugify }}">{{ k }}</a> ({{ count }})</li>
{%- endfor -%}
</ul>
<hr/>

{%- comment -%} Render each keyword bucket {%- endcomment -%}
{%- for k in keys -%}
  <h2 id="kw-{{ k | slugify }}">{{ k }}</h2>
  {%- assign bucket = "" | split: "" -%}
  {%- for p in docs -%}
    {%- if p.url != page.url and p.keywords -%}
      {%- assign raw = p.keywords -%}
      {%- if raw.first -%}{% assign arr = raw %}{% else %}{% assign arr = raw | split: ',' %}{% endif -%}
      {%- assign hit = false -%}
      {%- for kk in arr -%}
        {%- if kk | strip == k -%}{% assign hit = true %}{% break %}{% endif -%}
      {%- endfor -%}
      {%- if hit -%}
        {%- assign bucket = bucket | push: p -%}
      {%- endif -%}
    {%- endif -%}
  {%- endfor -%}
  {%- assign bucket = bucket | sort: "title" -%}
  <ul>
    {%- for p in bucket -%}
      <li><a href="{{ p.url | relative_url }}">{{ p.title | default: p.name }}</a></li>
    {%- endfor -%}
  </ul>
{%- endfor -%}
