---
layout: sidebar
title: 구의 기록
---

<div style="margin-bottom: 30px;">
  <img src="{{ '/assets/pale_blue_dot.jpg' | relative_url }}" alt="Pale Blue Dot" style="width: 100%; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.08); display: block;">
</div>

<hr style="margin: 35px 0 25px 0; border: none; border-top: 1px solid #eee;">

<h2 style="font-size: 1.35em; font-weight: 700; color: #222; margin-bottom: 20px;">최근 작성된 글</h2>

{%- assign all_docs = site.math | concat: site.genai | concat: site.note | concat: site.mml | concat: site.project | sort: "date" | reverse -%}
{%- include post-list.html posts=all_docs limit=7 -%}
