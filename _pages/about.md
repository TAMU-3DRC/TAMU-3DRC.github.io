---
permalink: /
title: "About 3DRC"
author_profile: false
layout: splash
classes: landing
redirect_from:
  - /about/
  - /about.html
---

<div class="timeline-controls">
  <input type="search" id="timeline-search" placeholder="Search timeline…" aria-label="Search timeline">
  <div id="timeline-filters">
    {% assign categories = site.data.timeline | map: 'category' | uniq | sort %}
    {% for cat in categories %}
      <label><input type="checkbox" value="{{ cat | downcase }}" checked> {{ cat }}</label>
    {% endfor %}
  </div>
</div>

<ul class="timeline">
  {% assign events = site.data.timeline | sort: 'date' %}
  {% for event in events %}
  <li class="timeline-item" data-category="{{ event.category | downcase }}">
    <div class="timeline-icon">
      <img src="{{ event.icon }}" alt="" onerror="this.onerror=null;this.src='/images/default-icon.svg';">
    </div>
    <div class="timeline-details">
      <h3>{{ event.title }}</h3>
      <p class="timeline-meta">
        <time datetime="{{ event.date }}">{{ event.date | date: '%Y-%m-%d' }}</time>
        • {{ event.category }}
      </p>
      <p>{{ event.summary }}</p>
      <details>
        <summary>More info</summary>
        <p>{{ event.description }}</p>
        {% if event.links %}
        <ul>
          {% for link in event.links %}
          <li><a href="{{ link.url }}">{{ link.label }}</a></li>
          {% endfor %}
        </ul>
        {% endif %}
      </details>
    </div>
  </li>
  {% endfor %}
</ul>

<style>
.timeline { list-style: none; margin: 2rem 0; padding-left: 2rem; border-left: 2px solid #ccc; }
.timeline-item { position: relative; margin-bottom: 2rem; }
.timeline-item::before { content: ''; position: absolute; left: -11px; top: 0.5rem; width: 10px; height: 10px; background: #fff; border: 2px solid #666; border-radius: 50%; }
.timeline-icon { position: absolute; left: -60px; top: 0; width: 40px; height: 40px; }
.timeline-icon img { width: 40px; height: 40px; object-fit: contain; }
.timeline-details { margin-left: 0; }
.timeline-meta { font-size: 0.9rem; color: #666; }
.timeline-controls { margin-bottom: 1rem; }
#timeline-filters label { margin-right: 0.5rem; }
</style>

<script>
(function() {
  const searchInput = document.getElementById('timeline-search');
  const filterBoxes = document.querySelectorAll('#timeline-filters input[type=checkbox]');
  const items = document.querySelectorAll('.timeline-item');

  function applyFilters() {
    const query = searchInput.value.toLowerCase();
    const active = Array.from(filterBoxes).filter(cb => cb.checked).map(cb => cb.value);
    items.forEach(item => {
      const text = item.innerText.toLowerCase();
      const cat = item.dataset.category;
      const matchText = !query || text.includes(query);
      const matchCat = active.includes(cat);
      item.style.display = matchText && matchCat ? '' : 'none';
    });
  }

  searchInput.addEventListener('input', applyFilters);
  filterBoxes.forEach(cb => cb.addEventListener('change', applyFilters));
})();
</script>
