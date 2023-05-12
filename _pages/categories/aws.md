---
layout: archive
permalink: aws
title: "AWS"
author_profile: false
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.aws %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
