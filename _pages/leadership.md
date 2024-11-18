---
layout: archive
title: "Leadership"
permalink: /leadership/
---

<div class="leadership-container">
  {% for leader in site.data.leadership %}
    <div class="flip-card">
      <div class="flip-card-inner">
        <div class="flip-card-front">
          <img src="{{ leader.image | relative_url }}" alt="{{ leader.name }}" class="leader-image">
          <h3>{{ leader.name }}</h3>
          <p>{{ leader.title }}</p>
        </div>
        <div class="flip-card-back">
          <p>{{ leader.bio }}</p>
          <a href="{{ leader.link }}" target="_blank">Read more</a>
        </div>
      </div>
    </div>
  {% endfor %}
</div>