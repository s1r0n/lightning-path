---
# While layouts are erroring, keep layout: null. After the theme loads, change to layout: default
layout: null
title: Keyword index
nav_order: 999
permalink: /keywords/
---

<h1>Keyword index</h1>
<p>
  Docs grouped by <code>keywords</code> front matter. Use either
  <code>keywords: [ai, pedagogy, brain]</code>
  or <code>keywords: ai, pedagogy, brain</code>.
</p>

{%- comment -%} Collect all candidate docs (pages + collections) {%- endcomment -%}
{%- assign docs = site.pages -%}
{%- for coll in site.collections -%}
  {%- if coll.docs -%}
    {%- assign docs = docs | concat: coll.docs -%}
  {%- endif -%}
{%- endfor -%}

{%- comment -%}
Normalize a page's keywords to an array of lowercase, trimmed tokens.
Handles:
- YAML arrays:            keywords: [ai, pedagogy]
- Comma-separated string: keywords: ai, pedagogy
- Single word string:     keywords: ai
{%- endcomment -%}
{%- assign all_keys = "" | split: "" -%}
{%- for p in docs -%}
  {%- if p.keywords -%}
    {%- assign raw = p.keywords -%}
    {%- assign tokens = "" | split: "" -%}
    {%- if raw contains "," -%}
      {%- assign tokens = raw | split: "," -%}
    {%- elsif raw.first -%}
      {%- comment -%} Array case {%- endcomment -%}
      {%- assign tokens = raw -%}
    {%- else -%}
      {%- comment -%} Single string => one token {%- endcomment -%}
      {%- assign tokens = raw | split: "|" -%}
      {%- unless tokens.size > 0 -%}{% assign tokens = raw | split: " " %}{% endunless -%}
      {%- if tokens.size == 0 -%}{% assign tokens = raw | split: ":" %}{% endif -%}
      {%- if tokens.size == 0 -%}{% assign tokens = raw | split: ";" %}{% endif -%}
      {%- if tokens.size == 0 -%}{% assign tokens = raw | split: "/" %}{% endif -%}
      {%- if tokens.size == 0 -%}{% assign tokens = raw | split: "," %}{% endif -%}
      {%- if tokens.size == 0 -%}{% assign tokens = raw | split: "" %}{% endif -%}
      {%- assign tokens = raw | split: "," -%} {# final fallback: treat as CSV #}
    {%- endif -%}

    {%- for t in tokens -%}
      {%- assign tclean = t | strip | downcase -%}
      {%- if tclean != "" -%}
        {%- assign all_keys = all_keys | push: tclean -%}
      {%- endif -%}
    {%- endfor -%}
  {%- endif -%}
{%- endfor -%}
{%- assign all_keys = all_keys | uniq | sort_natural -%}

{%- if all_keys.size == 0 -%}
<p><em>No keywords found. Add <code>keywords: [tag1, tag2]</code> to your pages’ front matter.</em></p>
{%- endif -%}

{%- comment -%} Mini TOC with counts {%- endcomment -%}
<ul>
{%- for k in all_keys -%}
  {%- assign count = 0 -%}
  {%- for p in docs -%}
    {%- if p.url != page.url and p.keywords -%}
      {%- assign raw = p.keywords -%}
      {%- assign arr = "" | split: "" -%}
      {%- if raw contains "," -%}{% assign arr = raw | split: "," %}
      {%- elsif raw.first -%}{% assign arr = raw %}
      {%- else -%}{% assign arr = raw | split: "," %}{% endif -%}
      {%- assign hit = false -%}
      {%- for kk in arr -%}
        {%- if kk | strip | downcase == k -%}{% assign hit = true %}{% break %}{% endif -%}
      {%- endfor -%}
      {%- if hit -%}{% assign count = count | plus: 1 %}{% endif -%}
    {%- endif -%}
  {%- endfor -%}
  <li><a href="#kw-{{ k | slugify }}">{{ k }}</a> ({{ count }})</li>
{%- endfor -%}
</ul>
<hr/>

{%- comment -%} Render each bucket (exact, case-insensitive match) {%- endcomment -%}
{%- for k in all_keys -%}
  <h2 id="kw-{{ k | slugify }}">{{ k }}</h2>
  {%- assign bucket = "" | split: "" -%}
  {%- for p in docs -%}
    {%- if p.url != page.url and p.keywords -%}
      {%- assign raw = p.keywords -%}
      {%- assign arr = "" | split: "" -%}
      {%- if raw contains "," -%}{% assign arr = raw | split: "," %}
      {%- elsif raw.first -%}{% assign arr = raw %}
      {%- else -%}{% assign arr = raw | split: "," %}{% endif -%}
      {%- assign hit = false -%}
      {%- for kk in arr -%}
        {%- if kk | strip | downcase == k -%}{% assign hit = true %}{% break %}{% endif -%}
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
