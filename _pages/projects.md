---
layout: projects
title: Projects
permalink: /projects/
---

<div class="projects">
  {% for project in site.data.projects %}
    <div class="project">
      <a href="{{ project.url }}" target="_blank">
        <img src="{{ project.image }}" alt="{{ project.name }}" class="project-image">
      </a>
      <div class="project-details">
        <p>{{ project.description }}</p>
      </div>
    </div>
  {% endfor %}
</div>
