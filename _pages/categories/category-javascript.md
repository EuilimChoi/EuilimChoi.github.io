---
title: "Javascript"
layout: archive
permalink: categories/javascript
author_profile: true
sidebar_main: true

sidebar:
  nav: "sidebar_main"
header:
  overlay_image: /assets/images/DSCF3606.JPG
  overlay_filter: 0.5
---

{% assign posts = site.categories['Javascript'] %} {% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
