---
layout: default
title: Publications
nav_order: 100
has_children: false
permalink: docs/Publications
---
<style>
  .conference-badge {
    display: inline-block;
    background-color: #eee;
    color: #444;
    font-weight: 600;
    font-size: 0.8rem;
    padding: 2px 6px;
    border-radius: 6px;
    margin-left: 0.5em;
    vertical-align: middle;
    border: 1px solid #ccc;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }
</style>

# Publications
# 2025

<ul class="pub-list">
  {% assign papers = site.pages | where: "parent", "Publications" | sort: "nav_order" %}
  {% for paper in papers %}
    <li style="margin-bottom: 1.8em;">
      <strong>
        <a href="{{ paper.project_url | default: paper.url | relative_url }}">{{ paper.title }}</a>
      </strong>
      {% if paper.conference %}
        <span class="conference-badge">{{ paper.conference }}</span>
      {% endif %}
      {% if paper.authors %}
        {% assign coworkers = paper.coworker | default: 0 | plus: 0 %}
        <div><em>
        {% for author in paper.authors %}
            {% if author == "Janghyeok Han" %}
                <strong>{{ author }}</strong>
            {% else %}
                {{ author }}
            {% endif %}

            {% if forloop.index0 < coworkers and 1 != coworkers %}
                <sup>†</sup>
            {% endif %}

            {% unless forloop.last %}, {% endunless %}
        {% endfor %}
        </em></div>
      {% endif %}
        <div>
            {% if paper.paper_url %}
                <a href="{{ paper.paper_url }}" target="_blank">[Paper]</a>
            {% endif %}
            {% if paper.project_url %}
                <a href="{{ paper.project_url }}" target="_blank">[Project]</a>
            {% endif %}
        </div>
    </li>
  {% endfor %}
</ul>