---
layout: default
title: Categories
permalink: /categories/
---

<h1>Categories</h1>

<ul class="category-tree">
  {% assign sorted_categories = site.categories | sort %}
  {% assign current_level1 = "" %}
  {% assign current_level2 = "" %}

  {% for cat in sorted_categories %}
    {% assign parts = cat[0] | split: "/" %}
    {% assign level1 = parts[0] %}
    {% assign level2 = parts[1] %}
    {% assign level3 = parts[2] %}

    {% if level1 != current_level1 %}
      {% if current_level1 != "" %}
        {% if current_level2 != "" %}</ul></li>{% endif %}
        </ul></li>
      {% assign current_level2 = "" %}
      {% endif %}

      <li class="level1-item">
        {% if level2 %}
          <span class="toggle" onclick="toggleCategory(this)">▶</span>
        {% endif %}
        <a href="{{ '/#' | append: level1 | relative_url }}">{{ level1 }}</a>
        <span class="post-count">({{ cat[1] | size }})</span>
        {% assign current_level1 = level1 %}
        {% if level2 %}
          <ul class="level2-list hidden">
        {% endif %}
    {% endif %}

    {% if level2 and level2 != current_level2 %}
      {% if current_level2 != "" %}</ul></li>{% endif %}
      <li class="level2-item">
        {% if level3 %}
          <span class="toggle" onclick="toggleCategory(this)">▶</span>
        {% endif %}
        <a href="{{ '/#' | append: level1 | append: '/' | append: level2 | relative_url }}">{{ level2 }}</a>
        <span class="post-count">({{ cat[1] | size }})</span>
        {% assign current_level2 = level2 %}
        {% if level3 %}
          <ul class="level3-list hidden">
        {% endif %}
    {% endif %}

    {% if level3 %}
      <li class="level3-item">
        <a href="{{ '/#' | append: level1 | append: '/' | append: level2 | append: '/' | append: level3 | relative_url }}">{{ level3 }}</a>
        <span class="post-count">({{ cat[1] | size }})</span>
      </li>
    {% endif %}
  {% endfor %}

  {% if current_level2 != "" %}</ul></li>{% endif %}
  {% if current_level1 != "" %}</ul></li>{% endif %}
</ul>

<style>
  .category-tree {
    list-style: none;
    padding-left: 0;
    line-height: 2;
  }

  .level1-item {
    margin-bottom: 0.5rem;
    font-weight: bold;
    font-size: 1.1rem;
  }

  .level1-item > a {
    color: #ff5c57;
  }

  .level2-list {
    list-style: none;
    padding-left: 2rem;
    margin-top: 0.5rem;
  }

  .level2-item {
    margin-bottom: 0.3rem;
    font-weight: 500;
    font-size: 1rem;
  }

  .level2-item > a {
    color: #007acc;
  }

  .level3-list {
    list-style: none;
    padding-left: 2rem;
    margin-top: 0.3rem;
  }

  .level3-item {
    margin-bottom: 0.2rem;
    font-weight: normal;
    font-size: 0.95rem;
  }

  .level3-item > a {
    color: #6c757d;
  }

  .post-count {
    color: #999;
    font-size: 0.85em;
    margin-left: 0.3rem;
    font-weight: normal;
  }

  .hidden {
    display: none;
  }

  .toggle {
    cursor: pointer;
    user-select: none;
    margin-right: 0.5rem;
    display: inline-block;
    width: 1rem;
    text-align: center;
    color: #666;
  }

  .toggle:hover {
    color: #333;
  }

  .category-tree a {
    text-decoration: none;
  }

  .category-tree a:hover {
    text-decoration: underline;
  }
</style>

<script>
  function toggleCategory(el) {
    const parent = el.parentElement;
    const submenu = parent.querySelector("ul");
    if (submenu) {
      submenu.classList.toggle("hidden");
      el.textContent = submenu.classList.contains("hidden") ? "▶" : "▼";
    }
  }
</script>
