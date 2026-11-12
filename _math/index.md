---
layout: sidebar
title: 수학
collection_name: math
nav_order: 0
permalink: /math/
---

{%- assign section_posts = site.math | sort: "date" | reverse -%}
{%- include post-list.html posts=section_posts limit=7 -%}
