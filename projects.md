---
layout: page
title: Meus Projetos
permalink: /projects/
---

<div class="projects-container">
  {% for project in site.projects %}
    <a href="{{ project.url | relative_url }}" class="project-card">
      
      <div class="project-header">
        <h3>{{ project.title }}</h3>
        <span class="project-status {{ project.status | slugify }}">
          {{ project.status }}
        </span>
      </div>

      <p class="project-desc">{{ project.description }}</p>

      <div class="project-tech">
        {% for tech in project.tech_stack %}
          <span class="tech-tag">{{ tech }}</span>
        {% endfor %}
      </div>
      
    </a>
  {% endfor %}
</div>