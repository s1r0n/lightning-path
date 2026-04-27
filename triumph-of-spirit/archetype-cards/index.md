---
title: Archetype Cards
layout: default
nav_order: 5
parent: Triumph of Spirit Archetype System
permalink: /lightning-path/triumph-of-spirit/archetype-cards/
---

# Archetype Cards

Browse the archetype card images below.

<div class="card-grid">
{% for file in site.static_files %}
  {% if file.path contains '/lightning-path/triumph-of-spirit/archetype-cards/' %}
    {% assign ext = file.extname | downcase %}
    {% if ext == '.jpg' or ext == '.jpeg' or ext == '.png' or ext == '.webp' or ext == '.gif' %}
      <div class="card-item">
        <a href="{{ file.path | relative_url }}">
          <img src="{{ file.path | relative_url }}" alt="{{ file.name | replace: ext, '' | replace: '-', ' ' | replace: '_', ' ' | capitalize }}" loading="lazy">
        </a>
        <p class="card-caption">{{ file.name | replace: ext, '' | replace: '-', ' ' | replace: '_', ' ' | capitalize }}</p>
      </div>
    {% endif %}
  {% endif %}
{% endfor %}
</div>

<style>
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
