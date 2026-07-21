---
layout: default
title: Projects
permalink: /projects/
---

<div class="projects-hub">
  <h1>:Buffer projects</h1>

  <div class="project-grid">
    {% for project in site.projects %}
      {% assign status_lower = project.status | downcase %}
      {% if status_lower != 'backlog' %}
        <div class="project-card">
          <h2 class="project-card-title" style="display: flex; align-items: center; justify-content: space-between; gap: 15px; flex-wrap: wrap;">
            <a href="{{ project.url | relative_url }}" style="margin: 0;">{{ project.title }}</a>
            {% if project.status %}
              <span class="status-badge {{ project.status | slugify }}">{{ project.status }}</span>
            {% endif %}
          </h2>

          <p class="project-excerpt">
            {{ project.brief }}
          </p>

          <div class="project-tags">
            {% for tag in project.tags %}
              <span class="tech-tag">{{ tag }}</span>
            {% endfor %}
          </div>
        </div>
      {% endif %}
    {% endfor %}
  </div>

  <hr class="section-divider" style="margin: 60px 0 40px 0; border: none; border-top: 1px dashed rgba(197, 176, 136, 0.2);">

  <h2 style="color: #C5B088; font-family: 'Fira Code', monospace; font-size: 1.5rem; margin-bottom: 20px;">:backlog</h2>

  <div class="project-grid" style="opacity: 0.6; filter: grayscale(20%);">
    {% for project in site.projects %}
      {% assign status_lower = project.status | downcase %}
      {% if status_lower == 'backlog' %}
        <div class="project-card">
          <h2 class="project-card-title" style="display: flex; align-items: center; justify-content: space-between; gap: 15px; flex-wrap: wrap;">
            <a href="{{ project.url | relative_url }}" style="margin: 0;">{{ project.title }}</a>
            <span class="status-badge backlog">Backlog</span>
          </h2>

          <p class="project-excerpt">
            {{ project.brief }}
          </p>

          <div class="project-tags">
            {% for tag in project.tags %}
              <span class="tech-tag">{{ tag }}</span>
            {% endfor %}
          </div>
        </div>
      {% endif %}
    {% endfor %}
  </div>

</div>-
