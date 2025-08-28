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

<section class="about-intro">
  <h1>About the 3D Drug Discovery and Development Center</h1>
  <p>
    Welcome to TAMU 3DRC! We’re dedicated to driving innovation at the intersection of
    biotechnology, engineering, and collaborative research. Explore our journey through
    the milestones that have shaped our mission and vision.
  </p>
</section>

<section class="timeline-section">
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
      <div class="timeline-content">
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
</section>

<style>
.about-intro {
  text-align: center;
  max-width: 800px;
  margin: 0 auto 3rem;
}
.about-intro h1 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}
.about-intro p {
  font-size: 1.15rem;
  color: #555;
}

.timeline-section { margin-top: 2rem; }
.timeline-controls {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  align-items: center;
  margin-bottom: 2rem;
}
#timeline-search {
  padding: 0.5rem 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}
#timeline-filters label {
  margin-right: 1rem;
  text-transform: capitalize;
}

.timeline {
  position: relative;
  list-style: none;
  margin: 0;
  padding-left: 0;
}
.timeline::before {
  content: '';
  position: absolute;
  left: 20px;
  top: 0;
  bottom: 0;
  width: 4px;
  background: #e0e0e0;
}
.timeline-item {
  position: relative;
  margin: 2rem 0;
  padding-left: 80px;
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.6s ease;
}
.timeline-item.show {
  opacity: 1;
  transform: none;
}
.timeline-icon {
  position: absolute;
  left: 0;
  top: 0;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #0a9396;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
.timeline-icon img {
  width: 24px;
  height: 24px;
  object-fit: contain;
}
.timeline-content {
  background: #f4f6f8;
  padding: 1rem 1.5rem;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.05);
}
.timeline-meta {
  font-size: 0.9rem;
  color: #666;
  margin-bottom: 0.5rem;
}

/* category accent colours */
.timeline-item[data-category='funding'] .timeline-icon { background: #0a9396; }
.timeline-item[data-category='milestone'] .timeline-icon { background: #3a5a40; }
.timeline-item[data-category='consortia'] .timeline-icon { background: #588157; }
.timeline-item[data-category='program'] .timeline-icon { background: #005f73; }
.timeline-item[data-category='training'] .timeline-icon { background: #94d2bd; }

@media (max-width: 600px) {
  .timeline-item { padding-left: 60px; }
  .timeline-icon { width: 30px; height: 30px; }
}
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

  const observer = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('show');
        observer.unobserve(entry.target);
      }
    });
  }, { threshold: 0.1 });

  items.forEach(item => observer.observe(item));
})();
</script>
