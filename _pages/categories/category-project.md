---
title: "What I done!"
layout: archive
permalink: categories/project
author_profile: true
sidebar_main: true

sidebar:
  nav: "sidebar_main"
header:
  overlay_image: /assets/images/DSCF3606.JPG
  overlay_filter: 0.5
---

{% assign posts = site.categories['PROJECT'] %} {% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
