---
layout: sidebar
title: 프로젝트
collection_name: project
nav_order: 0
permalink: /project/
---

{%- assign section_posts = site.project | sort: "date" | reverse -%}
{%- include post-list.html posts=section_posts limit=7 -%}
