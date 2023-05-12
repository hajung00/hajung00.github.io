---
layout: archive
permalink: git
title: "Git"
author_profile: false
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.git %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
