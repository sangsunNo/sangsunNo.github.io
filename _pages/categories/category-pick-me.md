---
title: "Pick me"
layout: archive
permalink: /categories/pickme
author_profile: true
sidebar_main: true
---


{% assign posts = site.categories['Pick me'] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}