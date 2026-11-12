---
layout: sidebar
title: 독립 항등 분포
collection_name: notes
nav_order: 3999
---

머신러닝과 통계적 추론의 가장 밑바탕에는 독립 항등 분포(independent and identically distributed, i.i.d.)라는 필수적인 가정이 깔려 있다. 수집된  $m$ 개의 데이터 표본 확률 변수  $Z\_1, Z\_2, \dots, Z\_m$  이 i.i.d.를 만족한다는 것은, 이 데이터들이 서로 어떠한 영향도 주고받지 않으며(Independent), 모두 완벽하게 동일한 하나의 확률 분포에서 생성되었다(Identically Distributed)는 것을 의미한다.  

## Identically Distributed (항등 분포)
"모든 확률 변수들이 동일한 분포에서 왔다"는 전제는 데이터 기반 학습이 성립하기 위한 가장 결정적인 조건이다.  
만약 데이터 집합 내의 각 확률 변수(sample)  $Z\_1, Z\_2, \dots, Z\_m$  이 각각 자신만의 고유한 확률 밀도 함수  $p\_1, p\_2, \dots, p\_m$  을 가진다고 가정해 보자. 우리는  $m$ 개의 데이터를 가지고  $m$ 개의 서로 다른 미지의 함수를 추정해야 하는 모순에 빠지며, 이는 학습 자체가 불가능함을 뜻한다.  
항등 분포를 가정한다는 것은 모든 확률 변수가 동일한 형태와 파라미터를 가진 단 하나의 전역적인 확률 밀도 함수  $p(\boldsymbol{z})$  에서 얻어졌다는 의미이다.  

<div class="math-container">
$$  
p_1(\boldsymbol{z}) = p_2(\boldsymbol{z}) = \dots = p_m(\boldsymbol{z}) = p(\boldsymbol{z})  
$$
</div>


이 전제가 확립됨으로써, 기계학습 모델은 수집된 수많은 개별 데이터  $\boldsymbol{z}\_1, \dots, \boldsymbol{z}\_m$  들을 파편화된 정보가 아니라 단 하나의 전역적인 함수  $p(\boldsymbol{z})$  를 찾아내기 위한 공동의 증거(Evidence)로 사용할 수 있게 된다.  

## Independent (독립)
여기서 '독립'이란 모든 데이터의 부분 집합이 서로 영향을 미치지 않는 상호 독립(Mutual Independence)을 지칭한다. 이 독립성이 전제되어야만, 전체 데이터셋이 관측될 결합 확률 밀도 함수를 개별 데이터의 주변 확률들의 단순한 곱으로 인수분해할 수 있다.  

<div class="math-container">
$$  
p(\boldsymbol{z}_1, \boldsymbol{z}_2, \dots, \boldsymbol{z}_m) = p(\boldsymbol{z}_1)p(\boldsymbol{z}_2)\cdots p(\boldsymbol{z}_m) = \prod_{i=1}^m p(\boldsymbol{z}_i)  
$$
</div>


만약 이 전제가 없다면, 우리는 첫 번째 데이터가 두 번째 데이터에 미치는 조건부 확률  $p(\boldsymbol{z}\_2 \mid \boldsymbol{z}\_1)$  과 같은 데이터 간의 복잡한 얽힘을 모두 계산해야만 한다.  
나아가 이 인수분해는 기계학습의 최적화 과정에 결정적인 기여를 한다. 0과 1 사이의 값인 확률값들을 계속 곱하게 되면 컴퓨터의 부동소수점은 언더플로우(Underflow)를 일으켜 결국 0으로 소멸하고 만다. 이를 방지하기 위해 단조 증가 함수인 자연로그를 취하면, 곱셈 연산이 덧셈 연산으로 치환된다.  

<div class="math-container">
$$  
\log p(\boldsymbol{z}_1, \dots, \boldsymbol{z}_m) = \log \left( \prod_{i=1}^m p(\boldsymbol{z}_i) \right) = \sum_{i=1}^m \log p(\boldsymbol{z}_i)  
$$
</div>


이 수식이 바로 신경망의 손실 함수를 계산할 때, 각 데이터의 오차(Log-likelihood)를 단순히 덧셈 기호( $\sum$ )로 누적할 수 있게 만드는 수학적 근거가 된다.  

## 경험적 측도와 종속성
모평균  $\boldsymbol{\mu} = \int \boldsymbol{z} p(\boldsymbol{z}) d\boldsymbol{z}$  와 모공분산  $\boldsymbol{\Sigma} = \int (\boldsymbol{z} - \boldsymbol{\mu})(\boldsymbol{z} - \boldsymbol{\mu})^\top p(\boldsymbol{z}) d\boldsymbol{z}$  은 본질적으로 기댓값 연산자  $\mathbb{E}$  를 사용한 연속 공간의 무한 적분 형태이다. 하지만 현실에서는 진짜 확률 밀도 함수  $p(\boldsymbol{z})$  를 알 수 없으므로 이 적분을 직접 계산하는 것은 불가능하다.  
때문에 수학에서는 '경험적 측도(Empirical Measure)'를 도입한다. i.i.d. 가정에 기반하여, 우리가 수집한  $m$ 개의 데이터 점 하나하나가 전체 확률 공간에서 정확히  $1/m$  의 균등한 발생 확률(확률 질량)을 가진다고 취급하는 것이다. 모든 데이터가 동일한 분포를 공유하므로 특정 데이터에 편가중치를 줄 근거가 없기 때문이다.  
하지만 이  $1/m$  이라는 가중치는 데이터가 **완벽하게 독립적일 때만** 그 수학적 정당성을 유지한다.  
극단적으로  $m-1$ 개의 확률 변수가 모두 첫 번째 데이터  $\boldsymbol{z}\_1$  과 똑같은 값을 가지도록 강하게 종속되어 있다고 가정해 보자. 데이터 100개를 모아  $1/100$ 을 곱해 평균이나 분산을 구하려 한다면, 실제 독립적인 정보량(유효 표본 수)은 단 1개 분량밖에 되지 않음에도 분모만 100으로 나누는 꼴이 된다.  
그 결과, 표본이 많아질수록 오차가 줄어들어야 한다는 대수의 법칙이 성립하지 않으며, 경험적 분산은 비정상적으로 0에 가깝게 축소된다. 즉, 데이터들이 서로 독립적이지 않다면  $1/m$  을 곱하여 구한 경험적 통계량은 모집단을 올바르게 대변하지 못한다.  

## 무한의 적분에서 유한의 합산으로
데이터가 i.i.d. 조건을 충족하여 경험적 측도가 정당성을 얻는 순간, 다루기 힘든 무한 연속 적분 연산자  $\int (\cdot) p(\boldsymbol{z})d\boldsymbol{z}$  는 아주 단순한 유한 이산 합산 연산자  $\frac{1}{m}\sum\_{i=1}^{m} (\cdot)$  으로 완벽하게 치환된다.  
그 결과 이론적인 모평균 공식은 자연스럽게 데이터의 산술 평균인 경험적 평균  $\bar{\boldsymbol{z}}$  로 도출된다.  

<div class="math-container">
$$  
\bar{\boldsymbol{z}} = \frac{1}{m} \sum_{i=1}^{m} \boldsymbol{z}_i  
$$
</div>


같은 맥락에서 모공분산 역시 외부의 기댓값 기호  $\mathbb{E}$  가  $\frac{1}{m}\sum$  으로 치환되고, 내부의 알 수 없는 모평균  $\boldsymbol{\mu}$  를 방금 구한 경험적 평균  $\bar{\boldsymbol{z}}$  로 대체함으로써 우리가 실제로 계산할 수 있는 경험적 공분산 공식으로 완성된다.  

<div class="math-container">
$$  
\hat{\boldsymbol{\Sigma}} = \frac{1}{m} \sum_{i=1}^{m} (\boldsymbol{z}_i - \bar{\boldsymbol{z}})(\boldsymbol{z}_i - \bar{\boldsymbol{z}})^\top  
$$
</div>
