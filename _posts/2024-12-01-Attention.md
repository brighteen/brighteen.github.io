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
---
위 Architecture에서 보이듯이 기존 RNN 모델에서는 Encoder에 마지막 Hidden State $$h_5$$로 구성된Context vecter $$C$$ 로부터 Decoder를 학습시키는데  
그럼 마지막 은닉층 이전 은닉층들의 정보는 비교적 덜 가지고 넘어가버리는 **정보 병목 현상**이 발생함.  
그래서 Encoder의 모든 $$h$$들과 Decoder의 관심 $$s$$의 Weight Sum을 통해 $$C$$를 재구성(Attention 도입).  
그치만 각각의 Hidden state $$h$$ 가 담고 있는 정보들이 평등하지 않음.  
이 말이 뭐냐하믄  
위 사진을 예시로 들면 `I love learning`라는 문장을 Tokenization해서 `I`, `love`, `learning` 3개의 Input 으로 넣었을때 각각에 대응되는 Hidden state를 $$(h_2, h_3, h_4)$$ 라고 했을때,  
$$h_4$$ 은 `learning` 라는 단어와 함께 `I`, `love` 의 정보를 담고 있지만 $$h_3$$ 는 `learning`의 정보를 담지 못하고 `love`와 `I`의 정보만 가지고 있음.  
why? 기존 Sequence한 data의 성격때문에.  
마찬가지로  $$h_2$$ 도 `I`의 정보밖에 없고 `love`와 `learning`의 정보를 담지 못함.  
그래서 각 $$h$$를 재구성하자. **어떻게**?  

## self-Attention
모든 $$h$$들 끼리 내적해서 나 자신을 포함한 모든 $$h$$ 의 정보를 가지고 있자.  
그러면 $$C$$(Context Vector)를 만들 때도 $$h$$들이 동등한 입장이 되는거 아니냐, 앞서 C를 재구성한 것처럼 모든 $$h$$들도 self로 Attention 해보자 해서 또 한번 $$h$$끼리 Weight Sum을 통해 재구성함.  
즉, 기존 RNN Model의 한계점인 **정보 병목현상**을 해결함과 데이터를 독립으로 보겠다.  
-> 데이터를 순서대로 처리하지 않고 '병렬 처리를 하겠다'가 가능해진것!  
그 결과 파라미터 $$W$$들을 업데이트하기 위해 Chain Rule에 의해 순차적으로 해당 $$h$$로 갔던 과정에서 모든 $$h$$를 거치지 않고 바로 해당 $$h$$로 갈 수가 있게 되면서 **계산량**이 확 줄어버림.  
-> **Vanishing Gradient**또한 해결.  
기존 RNN Model을 버릴 수 있는 새로운 모델이 나온거임.  
**BAAAM**

---

+ 작성하면서 드는 생각
1. 그럼 다른 CNN, FCN Model에서도 각 $$h$$들을 Weight Sum 해줘서 계산량 줄게 해주면 안되는건가?
  - FCN에서는 입력의 순서 정보가 없기에 Self-Attention을 적용하면 오히려 모델이 복잡해질 가능성이 큼.
  - CNN에서는 합성곱 필터를 사용해 국소적(Spatial)정보를 추출하는데 Self-Attention을 도입하면 계산량이 증가하거나 합성곱의 장점(지역적 학습)을 희석시킬 수 있음.
