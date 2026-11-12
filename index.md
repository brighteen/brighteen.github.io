---
layout: sidebar
title: 구의 기록
---

안녕하세요, 인공지능과 수학, 인지과학의 접점을 연구하고 기록하는 블로그입니다.   
머신러닝의 기초 수학부터 생성 모델의 학습 동역학, 그리고 뇌와 인공지능의 메커니즘을 탐구하는 생각들을 이곳에 담습니다.

<hr style="margin: 35px 0 25px 0; border: none; border-top: 1px solid #eee;">

<h2 style="font-size: 1.35em; font-weight: 700; color: #222; margin-bottom: 20px;">최근 작성된 글</h2>

{%- assign all_docs = site.math | concat: site.genai | concat: site.note | concat: site.mml | concat: site.project | sort: "date" | reverse -%}
{%- include post-list.html posts=all_docs limit=7 -%}
