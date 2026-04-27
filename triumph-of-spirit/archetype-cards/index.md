---
title: Archetype Cards
layout: default
nav_order: 5
parent: Triumph of Spirit Archetype System
permalink: /lightning-path/triumph-of-spirit/archetype-cards/
---

# Archetype Cards

Browse the archetype card images below.

{% assign image_files = "" | split: "" %}
{% assign target_path = "/lightning-path/triumph-of-spirit/archetype-cards/" %}

{% for file in site.static_files %}
  {% assign file_path = file.path %}
  {% assign ext = file.extname | downcase %}
  
  {% if file_path contains target_path %}
    {% if ext == ".jpg" or ext == ".jpeg" or ext == ".png" or ext == ".webp" or ext == ".gif" %}
      {% assign image_files = image_files | push: file %}
    {% endif %}
  {% endif %}
{% endfor %}

{% if image_files.size == 0 %}
> **No images found.** Ensure your image files are committed to `{{ target_path }}` and that Jekyll recognizes them as static files.
{% else %}
<div class="card-grid">
{% for file in image_files %}
  {% assign clean_name = file.name | replace: file.extname, "" | replace: "-", " " | replace: "_", " " | capitalize %}
  <div class="card-item">
    <a href="{{ file.path | relative_url }}">
      <img src="{{ file.path | relative_url }}" alt="{{ clean_name }}" loading="lazy">
    </a>
    <p class="card-caption">{{ clean_name }}</p>
  </div>
{% endfor %}
</div>
{% endif %}
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1.5rem;
  margin-top: 1.5rem;
}

.card-item {
  text-align: center;
  border: 1px solid #e1e4e8;
  border-radius: 6px;
  padding: 0.75rem;
  background: #fff;
}

.card-item img {
  max-width: 100%;
  height: auto;
  border-radius: 4px;
}

.card-caption {
  margin-top: 0.5rem;
  font-size: 0.875rem;
  color: #586069;
  word-break: break-word;
}
</style>
