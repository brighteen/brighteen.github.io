---
layout: post
title: 컴퓨터가 단어를 파악하는 방법
subtitle: 결국 이것도 서로 다른 벡터의 유사도를 계산하는 것임!!
date: 2024-12-02 11:30:00 +0900
categories: [dev]
---

# Tokenization
문장 : I like cake.
Tokenization : 'I', 'like', 'cake', '.'

Don't 같은 단어를 'Don', ''', 't'로 나눴을 때와 'Do', 'n't'으로 나눴을 때 각기 다른 결과를 도출하는 걸 염두에 두고 Tokenization해야함.  

## One - Hot Encoding
Token을 vector화 하는 가장 쉬운 방법.  
I = [1,0,0]  
like = [0,1,0]  
cake = [0,0,1]  

하지만 원 핫 인코딩으로는 **각 벡터가 다르다는 사실을 알려줄 뿐, 두 Token 간의 관계를 고려하지않음(설명하지 못함).**  
즉 관계가 있는 벡터(단어)들은 비교적 가깝게 위치해야 한다는 것.  
예를 들어 고양이, 개, 강아지 세개의 단어가 있으면 원핫인코딩으로는 서로 다른 단어 사이의 거리가 모두 **동일**함.  
-> 개와 강아지는 유사성을 띄기에 두 단어가 비교적 밀집(Dence)하게 존재해야 하는게 맞지 않냐는 생각.  

## Word2Vec
핵심은 **단어의 주변을 보면 그 단어을 안다.**
주어진 문장의 **맥락**에 따라 다음 단어를 예측하는 문제가 마치 **지도학습**과 유사함.  

### CBOW
I like cake.  
위 문장에서 'like'를 Center word(Target Word)로 두면 나머지 'I', 'cake'(Context Words)를 통해 Center word를 예측하는 방식.  

### Skip - Gram
CBOW와 반대로 Center Word('like')로 나머지 Context Words('I', 'cake')를 예측.  

## 결국 Embedding은
1. 고차원 데이터(원 핫 인코딩을 통해 얻은 벡터)를 보다 효율적인 저차원 벡터로 변환.
2. 의미적으로 유사한 데이터끼리 밀집되어 있자.
3. Embedding 레이어를 통해 모델이 데이터를 학습하며 벡터를 자동으로 최적화.

---
이후에 자연어처리(NLP)에서 Embedding은 고유한 입력 데이터를 연속된 벡터 공간으로 매핑하여 의미적, 구조적 정보를 표현함.
