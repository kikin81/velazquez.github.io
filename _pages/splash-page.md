---
title: "Francisco Velazquez"
layout: splash
permalink: /
date: 2020-12-09T20:00:00-00:00
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/splash_header.jpeg
excerpt: "Welcome to my personal site!"
intro:
  - excerpt: 'Nullam suscipit et nam, tellus velit pellentesque at malesuada, enim eaque. Quis nulla, netus tempor in diam gravida tincidunt, *proin faucibus* voluptate felis id sollicitudin. Centered with `type="center"`'
---

<h3 class="archive__subtitle">Recent Posts</h3>

<div class="feature__wrapper">
  <div class="feature__item--center">
    <div class="grid__wrapper">
      {% for post in site.posts limit:4 %}
        {% include archive-single.html type="grid" %}
      {% endfor %}
    </div>
  </div>
</div>

<!-- {% include feature_row id="intro" type="center" %} -->