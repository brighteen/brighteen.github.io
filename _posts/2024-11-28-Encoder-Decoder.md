---
layout: post
title: "Encoder-Decoder"
subtitle: transformer를 향해
date: 24-11-28 20:10:00 +0900
categories: [dev]
---

# Encoder-Decoder 구조란?

Input data를 받아 이를 다른 형태로 변환하는 **Neural Network Architecture**입니다. 이 구조는 기계 번역, 요약, 음성 인식 등 다양한 시퀀스 데이터를 처리하는 작업에서 널리 사용됨.  

### 1. Encoder
- 입력 데이터를 이해하는 단계.
- 주어진 Input sequence를 고정된 크기의 **잠재 표현(Latent Representation)**으로 변환.
- Input sequence를 처리하면서 중요한 정보를 압축해 하나의 vector(Context Vector)로 표현.

### 2. Decoder
- Encoder가 생성한 잠재 표현을 받아 원하는 출력 시퀀스를 생성.
- 예를 들어, 영어 문장을 입력하면 디코더는 이를 스페인어 문장으로 변환.
- 출력은 순차적으로 생성되며, 이전 출력이 다음 출력의 생성을 도움.(RNN의 특징)

## 동작 원리

1. **Encoding**
   - 입력 데이터(예: 문장)를 vector로 변환.(Word Embedding)
   - RNN, LSTM, GRU 또는 Transformer 같은 네트워크를 사용하여 sequence data를 처리.

2. **Decoding**
   - 인코더의 출력을 기반으로 목표 출력(예: 번역된 문장)을 생성.
   - 이 단계에서도 순환 구조나 어텐션 메커니즘이 활용.

## 특징
- 입력과 출력의 길이가 달라도 작동 가능: "시퀀스-투-시퀀스(Seq2Seq)" 문제 해결에 적합.
- **어텐션 메커니즘**과 결합하면, 입력 전체에서 중요한 정보를 더 잘 활용할 수 있음.

## 예시: 기계 번역
- 입력: `I love you.`
- 인코더 출력: 잠재 표현(컨텍스트 벡터)
- 디코더 입력: 잠재 표현 + 이전 출력
- 출력: `Je t'aime.`

## 한계점
- 고정된 크기의 벡터로 입력 정보를 압축해야 하므로, 입력 길이가 길어질수록 정보 손실 가능성이 있음.
- 이를 해결하기 위해 **어텐션 메커니즘**이 등장함.

## 정리
Encoder-Decoder 구조는 입력 데이터를 잠재 표현으로 변환하고, 이를 기반으로 출력 데이터를 생성하는 강력한 아키텍처입니다. 어텐션 메커니즘과 결합하여 더 높은 성능을 발휘하며, 자연어 처리(NLP)와 같은 분야에서 매우 유용하게 사용되고 있습니다.
