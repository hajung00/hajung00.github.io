---
layout: archive
permalink: aws
title: "AWS"
author_profile: false
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.aws %}
{% for post in site.posts %}
{% include custom-archive-single.html type=entries_layout %}
{% endfor %}
