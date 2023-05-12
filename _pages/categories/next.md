---
layout: archive
permalink: next
title: "Next.js"
author_profile: false
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.next %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
