---
layout: default
title: Projects
permalink: /projects/
---

<div class="projects-hub">
  <h1>Projects</h1>

  <div class="project-grid">
    {% for project in site.projects %}
      <div class="project-card">
        <h2 class="project-card-title">
          <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
        </h2>

        <p class="project-excerpt">
          {{ project.excerpt | strip_html | truncatewords: 25 }}
          {% if project.status %}
            <span class="status-badge {{ project.status | slugify }}">{{ project.status }}</span>
          {% endif %}
        </p>

        <div class="project-tags">
          {% for tag in project.tags %}
            <span class="tech-tag">{{ tag }}</span>
          {% endfor %}
        </div>
      </div>
    {% endfor %}
  </div>
</div>
