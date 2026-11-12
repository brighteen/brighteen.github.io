---
layout: sidebar
title: 노트
collection_name: note
nav_order: 0
permalink: /note/
---

{%- assign section_posts = site.note | sort: "date" | reverse -%}
{%- include post-list.html posts=section_posts limit=7 -%}
