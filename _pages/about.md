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

<img src="/images/tmc3-crb.jpg" alt="TMC3 campus" class="about-banner">

<section class="about-intro">
  <h1>The Drug Discovery and Development Resource Center</h1>
  <p>
  Welcome to the Texas A&M Drug Discovery & Development Resource Center (3DRC)—your gateway to translational science at the heart of the Texas Medical Center. Anchored within the Institute of Biosciences & Technology (IBT), we connect investigators with a region-wide ecosystem of advanced technology cores and the Gulf Coast Consortia (GCC) to accelerate ideas from target to therapeutic. Our capabilities span large-scale virtual screening and generative ligand design, high-throughput biochemical assays, quantitative binding kinetics analysis, cell-based screening for hit discovery and validation, microphysiological systems for lead optimization, and commercialization support.  
  </p>
  
  <p>
  Explore our journey and milestones that reflect our mission: to advance innovative therapies through collaborative science, rigorous methods, and access to best-in-class instrumentation and expertise.
  </p>
  
</section>

<section class="timeline-section">
  <div class="timeline-controls">
    <input type="search" id="timeline-search" placeholder="Search timeline…" aria-label="Search timeline">
    <div id="timeline-filters" class="filter-buttons">
      {% assign categories = site.data.timeline | map: 'categories' | join: ',' | split: ',' | uniq | sort %}
      {% for cat in categories %}
        <button class="filter-btn active" data-category="{{ cat | downcase }}" aria-pressed="true">
          <span class="icon"><img src="/images/{{ cat | downcase }}.svg" alt="{{ cat }}"></span>
          <span class="label">{{ cat }}</span>
        </button>
      {% endfor %}
    </div>
  </div>

  <ul class="timeline">
    {% assign events = site.data.timeline | sort: 'date' %}
    {% for event in events %}
    <li class="timeline-item" data-category="{{ event.categories | join: ' ' | downcase }}">
      <div class="timeline-icon">
        <img src="{{ event.icon }}" alt="" onerror="this.onerror=null;this.src='/images/default-icon.svg';">
      </div>
      <div class="timeline-content">
        <h3>{{ event.title }}</h3>
        <p class="timeline-meta">
          <time datetime="{{ event.date }}">{{ event.date | date: '%Y-%m-%d' }}</time>
          • {{ event.categories | join: ', ' }}
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
  text-align: justify;
  width: 100%;
  margin: 0 0 3rem;
}
.about-intro h1 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
  text-align: center;
}
.about-intro p {
  font-size: 1.15rem;
  color: #707373;
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
  border: 1px solid #707373;
  border-radius: 4px;
}
#timeline-filters {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}
.filter-btn {
  display: flex;
  align-items: center;
  gap: 0.25rem;
  padding: 0.4rem 0.6rem;
  border: 1px solid #707373;
  border-radius: 4px;
  background: #f0f0f0;
  cursor: pointer;
  text-transform: capitalize;
}
.filter-btn .icon img {
  width: 1.1rem;
  height: 1.1rem;
}
.filter-btn.active .icon img {
  filter: invert(1);
}
.filter-btn.active {
  background: #500000;
  color: #fff;
  border-color: #500000;
}
.about-banner {
  width: 100%;
  height: auto;
  display: block;
  margin-bottom: 2rem;
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
  background: #500000;
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
  color: #707373;
  margin-bottom: 0.5rem;
}

/* category accent colours */
.timeline-item[data-category~='funding'] .timeline-icon { background: #500000; }
.timeline-item[data-category~='milestone'] .timeline-icon { background: #707373; }
.timeline-item[data-category~='consortia'] .timeline-icon { background: #500000; }
.timeline-item[data-category~='program'] .timeline-icon { background: #707373; }
.timeline-item[data-category~='training'] .timeline-icon { background: #500000; }

@media (max-width: 600px) {
  .timeline-item { padding-left: 60px; }
  .timeline-icon { width: 30px; height: 30px; }
}
</style>

<script>

(function() {
  const searchInput = document.getElementById('timeline-search');
  const filterButtons = document.querySelectorAll('#timeline-filters button');
  const items = document.querySelectorAll('.timeline-item');

  function applyFilters() {
    const query = searchInput.value.toLowerCase();
    const active = Array.from(filterButtons)
      .filter(btn => btn.classList.contains('active'))
      .map(btn => btn.dataset.category);
    items.forEach(item => {
      const text = item.innerText.toLowerCase();
      const cats = item.dataset.category.split(' ');
      const matchText = !query || text.includes(query);
      const matchCat = active.some(cat => cats.includes(cat));
      item.style.display = matchText && matchCat ? '' : 'none';
    });
  }

  searchInput.addEventListener('input', applyFilters);
  filterButtons.forEach(btn => {
    btn.addEventListener('click', () => {
      btn.classList.toggle('active');
      btn.setAttribute('aria-pressed', btn.classList.contains('active'));
      applyFilters();
    });
  });

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
