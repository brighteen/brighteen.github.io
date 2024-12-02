---
layout: post
title: Attention과 self-Attention
subtitle: 결국 모든 것은 Vector의 유사도를 구하는 것이다!
date: 2024-12-01 00:10:00 +0900
categories: [dev]
---

기존 RNN의 sequence한 data를 처리하는데 **계산량**을 self-Attention을 통해 해결.  
**Vanishing Gradient**도 역시 해결!  
그럼 어떻게? How?  

## 기존 RNN 모델  
![RNN Model](/assets/images/Encoder_Decoder.png)  
Encoder에서 Context vecter $C$ =마지막 은닉층 $h$ 를 가지고 Decoder를 학습시키는데  
그럼 마지막 은닉층 전의 은닉층들의 정보는 비교적 덜 가지고 넘어가버리는 **정보 병목 현상**이 발생.  
그래서 $C$를 모든 은닉상태 $h$를 고려해서 재구성(Attention 도입).  
그치만 각각의 Hidden state $h$가 담고 있는 정보들이 평등하지 않음.  
이 말이 뭐냐하믄  
예를 들어 `I like cake`라는 문장을 토큰해서 `I`, `like`, `cake` 3개의 인풋으로 넣었을때 각각에 대응되는 Hidden state를 $(h_1, h_2, h_3)$ 라고 했을때,  
$h_3$ 은 `cake` 라는 단어와 함께 `I`, `like` 의 정보를 담고 있지만 $h_2$ 는 `cake`의 정보를 담지 못하고 `like`와 `I`의 정보만 가지고 있음.  
why? 기존 Sequence한 data의 성격때문에.  
마찬가지로  $h_1$ 도 `I`의 정보밖에 없고 `like`와 `cake`의 정보를 담지 못함.  
그래서 각 $h$를 재구성하자. **어떻게**?  

## self-Attention
$h_1$ 를 모든 $h$ 와 내적해서 나 자신을 포함한 모든 $h$ 의 정보를 가지고 있자.  
그러면 $C$(Context Vector)를 만들 때도 $h$들이 동등한 입장이 되는거 아니냐 해서 모든 $h$를 weight sum을 통해 재구성함.  
즉, 앞서 얘기한 정보 `병목현상을 해결함`과 데이터를 독립으로 보겠다 -> '병렬 처리를 하겠다'를 선언하면서 순차적인 처리를 해서 생기는 엄청난 **계산량**과 그에 따른 미분을 통해 발생하는 **Vanishing Gradient**또한 해결한 것.  
이 말은 기존 RNN Model을 버릴 수 있는 새로운 모델이 나온거임.  
**BAAAM**
