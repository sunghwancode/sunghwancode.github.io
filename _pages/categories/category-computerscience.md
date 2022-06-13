---
title: "ComputerScience"
layout: archive
permalink: categories/computerscience
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.ComputerScience %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}
