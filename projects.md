---
layout: default
title: Projects
permalink: /projects/
---

<div class="projects-hub">
  <h1>:Buffer projects</h1>

  <!-- SEÇÃO 1: Projetos Ativos -->
  <div class="project-grid">
    {% for project in site.projects %}
      <!-- Filtra tudo que NÃO for "Backlog" -->
      {% assign status_lower = project.status | downcase %}
      {% if status_lower != 'backlog' %}
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
      {% endif %}
    {% endfor %}
  </div>

  <!-- DIVISÓRIA SEMÂNTICA -->
  <hr class="section-divider" style="margin: 60px 0 40px 0; border: none; border-top: 1px dashed rgba(197, 176, 136, 0.2);">

  <!-- SEÇÃO 2: Backlog -->
  <h2 style="color: #C5B088; font-family: 'Fira Code', monospace; font-size: 1.5rem; margin-bottom: 20px;">:backlog</h2>

  <div class="project-grid" style="opacity: 0.6; filter: grayscale(20%);">
    {% for project in site.projects %}
      <!-- Filtra estritamente o que FOR "Backlog" -->
      {% assign status_lower = project.status | downcase %}
      {% if status_lower == 'backlog' %}
        <div class="project-card">
          <h2 class="project-card-title">
            <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
          </h2>

          <p class="project-excerpt">
            {{ project.excerpt | strip_html | truncatewords: 25 }}
            <span class="status-badge backlog">Backlog</span>
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

</div>
