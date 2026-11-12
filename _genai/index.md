---
layout: sidebar
title: 생성모델
collection_name: genai
nav_order: 0
permalink: /genai/
---

{%- assign section_posts = site.genai | sort: "date" | reverse -%}
{%- include post-list.html posts=section_posts limit=7 -%}
