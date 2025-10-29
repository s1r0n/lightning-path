---
layout: default
title: Keyword index
nav_order: 999
permalink: /keywords/
---

<h1>Keyword index</h1>
<p>Docs grouped by <code>keywords</code> front matter (array form only).</p>

{%- comment -%} Gather all pages + collection docs {%- endcomment -%}
{%- assign docs = site.pages -%}
{%- for coll in site.collections -%}
  {%- if coll.docs -%}
    {%- assign docs = docs | concat: coll.docs -%}
  {%- endif -%}
{%- endfor -%}

{%- comment -%} Build the unique keyword list (arrays only) {%- endcomment -%}
{%- assign keys = "" | split: "" -%}
{%- for p in docs -%}
  {%- if p.url == page.url or p.url contains "/assets/" -%}{% continue %}{%- endif -%}
  {%- if p.keywords and p.keywords.first -%}
    {%- for k in p.keywords -%}
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

{%- comment -%} Mini TOC with per-key counts {%- endcomment -%}
<ul>
  {%- for k in keys -%}
    {%- assign count = 0 -%}
    {%- for p in docs -%}
      {%- if p.url == page.url or p.url contains "/assets/" -%}{% continue %}{%- endif -%}
      {%- if p.keywords and p.keywords.first -%}
        {%- if p.keywords contains k -%}
          {%- assign count = count | plus: 1 -%}
        {%- endif -%}
      {%- endif -%}
    {%- endfor -%}
    <li><a href="#kw-{{ k | slugify }}">{{ k }}</a> ({{ count }})</li>
  {%- endfor -%}
</ul>
<hr/>

{%- comment -%} Render each bucket {%- endcomment -%}
{%- for k in keys -%}
  <h2 id="kw-{{ k | slugify }}">{{ k }}</h2>
  {%- assign bucket = "" | split: "" -%}
  {%- for p in docs -%}
    {%- if p.url == page.url or p.url contains "/assets/" -%}{% continue %}{%- endif -%}
    {%- if p.keywords and p.keywords.first and p.keywords contains k -%}
      {%- assign bucket = bucket | push: p -%}
    {%- endif -%}
  {%- endfor -%}
  {%- assign bucket = bucket | sort: "title" -%}
  <ul>
    {%- for p in bucket -%}
      <li><a href="{{ p.url | relative_url }}">{{ p.title | default: p.name }}</a></li>
    {%- endfor -%}
  </ul>
{%- endfor -%}
