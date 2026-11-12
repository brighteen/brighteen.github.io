---
layout: sidebar
title: KL Divergence
collection_name: notes
nav_order: 5999
---

# 쿨백-라이블러 발산 (KL Divergence)과 변분 추론

두 확률 분포의 차이(정보 손실량)를 재는 척도(measure) 중 하나로 쿨백-라이블러 발산(Kullback-Leibler Divergence, KL 발산)이 있다.  

KL 발산은 두 확률 분포  $P$ 와  $Q$ 가 있을 때, 분포  $P$ 를 설명하기 위해 분포  $Q$ 를 대신 사용하면 발생하는 정보의 손실량(비효율성)을 측정한다. 이때  $P$ 는 우리가 도달하고자 하는 진짜 분포(True Distribution)이고,  $Q$ 는 우리가 관측된 데이터로 추정한 근사 분포이다. 만약  $P$ 와  $Q$ 가 완벽히 같다면, 정보 손실은 0이 된다.  

KL 발산은 이산형(discrete) 데이터와 연속형(continuous) 데이터에 대해 각각 다음과 같이 정의된다.  

<div class="math-container">
$$  
D_{KL}(P \parallel Q) = \sum_{x} P(x) \log \frac{P(x)}{Q(x)}  
$$
</div>



<div class="math-container">
$$  
D_{KL}(P \parallel Q) = \int P(x) \log \frac{P(x)}{Q(x)} dx  
$$
</div>


이 수식은  $P$ 와  $Q$ 의 비율에 로그를 씌운 값의 기댓값(평균)을 구하는 것이다. 여기서 분포의 비율  $\frac{P(x)}{Q(x)}$ 를 계산한다는 것은, 공간상의 '정확히 동일한 좌표 위치  $x$ '에서 정답 함수  $P$ 의 확률 밀도(높이)와 추정 함수  $Q$ 의 확률 밀도(높이)의 상대적 비율을 계산한다는 뜻이다. (예를 들어 특정 좌표에서  $P$ 의 밀도가 0.8이고  $Q$ 의 밀도가 0.2라면, 그 비율은 4가 되며 이 차이에 로그를 씌워 정보 손실로 합산한다.)  



<div class="obsidian-callout" markdown="1">
P를 모르는데 특정 위치에서 P의 확률 밀도를 어떻게 계산하는가?
</div>



이산형 데이터에 대한 KL 발산은 로그의 성질을 이용해 다음과 같이 분해할 수 있다.  

<div class="math-container">
$$  
D_{KL}(P \parallel Q) = \sum P(x) \log P(x) - \sum P(x) \log Q(x)  
$$
</div>


이는  $P$ 와  $Q$ 의 크로스 엔트로피에서  $P$ 의 엔트로피(어떤 상수)를 뺀 것과 같다.  

<div class="math-container">
$$  
D_{KL}(P \parallel Q) = H(P, Q) - H(P)  
$$
</div>


크로스 엔트로피는 보통 분류 문제에서 손실 함수로 사용된다. 즉, 크로스 엔트로피를 최소화한다는 것은 우리가 다룰 수 없는 상수  $H(P)$ 는 무시하고, 두 분포 간의 KL 발산을 최소화하여  $Q$ 를  $P$ 에 근사시키겠다는 의미이다.  

## KL 발산의 비대칭성과 최적화 방향

KL 발산은 항상  $D\_{KL}(P \parallel Q) \ge 0$ 이 성립하며, 오직  $P$ 와  $Q$ 가 완벽히 같을 때만 0이 된다. 종종 KL 발산을 '두 분포 간의 거리'라고 부르지만, 수학적 거리(distance)와 달리 KL 발산은 비대칭성(Asymmetry)을 가진다.  

<div class="math-container">
$$  
D_{KL}(P \parallel Q) \neq D_{KL}(Q \parallel P)  
$$
</div>


이러한 비대칭성 때문에 수식을 어느 방향으로 쓰느냐에 따라  $Q$ 가  $P$ 에 근사하는 방식이 완전히 달라진다. 만약  $P$ 가 두 개의 봉우리를 가지는 다봉 분포(bimodal)라고 가정해 보자.  

1. Forward KL:  $D\_{KL}(P \parallel Q)$ 

<div class="math-container">
$$  
D_{KL}(P \parallel Q) = \int P(x) \log \frac{P(x)}{Q(x)} dx = \mathbb{E}_{x \sim P} \left[ \log \frac{P(x)}{Q(x)} \right]  
$$
</div>


이 경우  $P(x)$ 가 큰데  $Q(x)$ 가 0에 가까우면  $\log \frac{P(x)}{Q(x)} \to \infty$ 가 되어 무한대 페널티가 발생한다. 반면  $P(x)$ 가 0에 가까우면  $Q(x)$ 가 커도 앞에 곱해진  $P(x)$ 로 인해 전체 페널티는 0이 된다.  
따라서 모델  $Q$ 는 무한대 페널티를 피하기 위해  $P(x) > 0$ 인 모든 구역에 대해 반드시 확률 밀도를 가져야 한다. 즉,  $Q$ 의 지지 집합(support)은  $P$ 의 지지 집합을 완전히 포함해야 한다 ( $supp(P) \subseteq supp(Q)$ ).  
만약  $P$ 가 다봉 분포일 때 단봉 분포인  $Q$ 로 이를 근사하려 한다면,  $Q$ 는  $P$ 의 모든 봉우리를 덮기 위해  $P(x)=0$ 인 계곡 구역에 불필요한 확률 질량을 배치하는 것을 감수하며 넓게 퍼진다. 이를 mean-seeking(평균 추구) 또는 zero-avoiding이라고 한다.  

2. Reverse KL:  $D\_{KL}(Q \parallel P)$   

<div class="math-container">
$$  
D_{KL}(Q \parallel P) = \int Q(x) \log \frac{Q(x)}{P(x)} dx = \mathbb{E}_{x \sim Q} \left[ \log \frac{Q(x)}{P(x)} \right]  
$$
</div>


이번에는  $Q(x)$ 가 큰데  $P(x)$ 가 0에 가까우면 분모  $P(x)$ 로 인해  $\log \frac{Q(x)}{P(x)} \to \infty$ 가 되어 페널티가 폭발한다. 반대로  $Q(x)$ 가 0에 가까울 때는 페널티가 0이 된다.  
따라서 모델  $Q$ 는 무한대 페널티를 피하기 위해  $P(x)$ 가 0인 구역에서는 자신도 반드시 0이 되어야 한다. 즉,  $Q$ 의 지지 집합은  $P$ 의 지지 집합 내부에 완전히 속해야 한다 ( $supp(Q) \subseteq supp(P)$ ).  
만약  $P$ 가 다봉 분포일 때,  $Q$ 는  $P(x)=0$ 인 구역을 가로지르는 것을 극도로 회피한다. 그 결과  $Q$ 는 여러 봉우리를 덮는 것을 포기하고, 가장 확률 질량이 높은 단 하나의 봉우리에만 집중적으로 몰리며 분산이 극도로 좁아진다. 이를 mode-seeking 또는 zero-forcing이라고 부른다.  
(_참고: mode-seeking 현상은 Reverse KL의 '0을 피하려는 성질'과  $Q$ 를 '단일 가우시안'으로 제한한 구조적 한계가 결합하여 나타난다. 만약  $Q$ 를 혼합 가우시안처럼 매우 유연하게 가정한다면,  $P$ 가 다봉 분포여도  $Q$ 가 여러 mode를 동시에 학습할 수 있다._)  

## 변분 추론 (Variational Inference)과 ELBO

기계 학습의 생성 모델(예: VAE)에서는 잠재 공간(Latent Space)을 다루기 위해 KL 발산을 활용한다. 먼저 이 맥락에서 사용되는 용어와 표기를 엄밀히 정의하면 다음과 같다.  
- ** $P(Z)$ **: 아무 데이터( $X$ )도 보지 않았을 때 잠재 공간  $Z$ (데이터 생성의 원인)가 가질 것이라 가정하는 사전 확률(Prior). (보통  $\mathcal{N}(0, I)$ 로 가정)
- ** $P(Z \mid X)$ **: 특정 관측 데이터  $X$ 가 주어졌을 때 잠재 공간  $Z$ 가 가져야 하는 진짜 사후 확률(True Posterior). 우리가 도달하고 싶지만 계산할 수 없는 True 분포이다.
- ** $Q(Z \mid X)$ **: 다루기 힘든  $P(Z \mid X)$ 를 근사하기 위해 신경망(인코더)으로 만든 근사 사후 확률(Approximate Posterior).

우리는 True 분포인  $P(Z \mid X)$ 를 모르기 때문에  $P$ 를 가중치로 두는 Forward KL의 기댓값을 구할 수 없다. 따라서 우리가 통제할 수 있는 분포  $Q(Z \mid X)$ 를 가중치로 두는 Reverse KL을 사용하여 적분을 가능하게 만들어야 한다.  
하지만 Reverse KL  $D\_{KL}(Q(Z \mid X) \parallel P(Z \mid X))$ 의 식 내부에도 여전히 모르는  $P(Z \mid X)$ 가 포함되어 있다. 다행히도, 이 값을 직접 계산하지 않고도 KL 발산을 최소화하는 대수학적 우회로가 존재한다. 베이즈 정리( $P(Z \mid X) = \frac{P(X, Z)}{P(X)}$ )를 이용해 수식을 분해하면 다음과 같다.  

<div class="math-container">
$$  
D_{KL}(Q(Z \mid X) \parallel P(Z \mid X)) = \mathbb{E}_{Z \sim Q(Z \mid X)} \left[ \log Q(Z \mid X) - \log \frac{P(X, Z)}{P(X)} \right]  
$$
</div>


로그의 성질을 이용해 분리하면,  $\log P(X)$ 는 기댓값 연산 변수  $Z$ 와 무관한 상수이므로 밖으로 빠져나온다.  

<div class="math-container">
$$  
D_{KL}(Q(Z \mid X) \parallel P(Z \mid X)) = \mathbb{E}_{Z \sim Q} [\log Q(Z \mid X) - \log P(X, Z)] + \log P(X)  
$$
</div>


양변을 우리가 구하고자 했던 상수  $\log P(X)$ 에 대해 정리하면 다음과 같은 위대한 공식이 도출된다.  

<div class="math-container">
$$  
\log P(X) = \underbrace{\mathbb{E}_{Z \sim Q} [\log P(X, Z) - \log Q(Z \mid X)]}_{\text{ELBO}} + \underbrace{D_{KL}(Q(Z \mid X) \parallel P(Z \mid X))}_{\text{우리가 줄이고 싶은 목표 (항상 0 이상)}}  
$$
</div>


여기서 좌변의  $\log P(X)$ 가 고정되어 있다는 것은 이것이 우주의 절대적인 상수라는 뜻이 아니다. "우리가 최적화하려는 주체인  $Q$ (신경망)의 관점에서는 무관하다"는 의미다.  $X$ 는 이미 관측된 데이터이므로,  $Q$ 의 파라미터(신경망 가중치  $\phi$ )를 어떻게 바꾸든 이미 주어진 데이터의 주변 확률  $\log P(X)$ 는 변하지 않는다 ( $\frac{\partial}{\partial \phi} \log P(X) = 0$ ).  

결론적으로, 좌변이 고정되어 있고 KL 발산이 항상 0 이상이라면, 수식 우변에 있는 ELBO(Evidence Lower Bound) 항을 최대화하는 것은 곧 우리가 그토록 원했던 KL 발산 항을 최소화하는 것과 완벽하게 동일한 결과를 낳는다. ELBO 내부에는 우리가 계산할 수 있는 결합 확률  $P(X, Z)$ 와 근사 분포  $Q(Z \mid X)$ 만 존재하므로, 비로소 최적화가 가능해진다.  



<div class="obsidian-callout" markdown="1">
 $P(X, Z)$ 와  $Q(Z \mid X)$ 는 어떻게 계산하는가?

ELBO항을 좀 더 분해해보자. 결합 확률  $P(X, Z)$ 를 확률의 연쇄 법칙을 이용해 조건부 확률과 주변 확률의 곱인  $P(X \mid Z)P(Z)$ 로 분해된다.

<div class="math-container">
$$
\text{ELBO} = \mathbb{E}_{Z \sim Q} [\log P(X \mid Z) + \log P(Z) - \log Q(Z \mid X)]
$$  
</div>


기댓값이 선형 연산자이므로

<div class="math-container">
$$
\text{ELBO} = \underbrace{\mathbb{E}_{Z \sim Q} [\log P(X \mid Z)]}_{\text{1. 재구성 오차 (Reconstruction Loss)}} - \underbrace{D_{KL}(Q(Z \mid X) \parallel P(Z))}_{\text{2. 정규화 항 (Regularization)}}
$$  
</div>


이것이 실제 VAE가 최적화하는 목적 함수이다. 이 세 분포의 계산 과정을 보자.

 $P(Z)$ 는 보통 표준 정규 분포 $\mathcal{N}(0, \boldsymbol{I})$ 로 가정한다. 이는 기하학적으로  $Z$ 공간이 평균이 0이고 분산이 1인 공간임을 바라는 것과 같다. 어떤  $Z$ 값이 주어지면, 가우시안에 대입하여  $\log P(Z)$ 를 계산할 수 있다.

 $Q(Z \mid X)$ 는 근사 사후 확률로 인코더이다. 이는 단일 가우시안 분포  $\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\sigma}^2\boldsymbol{I})$ 로 가정하며, 이때 파라미터( $\boldsymbol{\mu}, \boldsymbol{\sigma}^2$ )는 고정된 값이 아닌 입력 데이터  $X$ 를 받아  $\boldsymbol{\mu}$ 와  $\boldsymbol{\sigma}^2$ 를 출력하는 인코더를 통해 계산한다.

<div class="math-container">
$$
[\boldsymbol{\mu}, \log \boldsymbol{\sigma}^2] = \text{Encoder}_{\phi}(X)
$$  
</div>


관측된 데이터  $X$ 를 인코더의 입력으로 넣어 특정  $\boldsymbol{\mu}$ 와  $\boldsymbol{\sigma}$ 벡터를 얻은 다음, 두 벡터를 모수로 가지는 가우시안 분포에서 잠재 변수  $Z$ 를 하나 샘플링한다(이때 역전파를 위해 reparameterization trick) 불확실성을 다른 확률 변수  $\boldsymbol{\epsilon}$ 를 도입한다)

<div class="math-container">
$$
Z = \boldsymbol{\mu} + \boldsymbol{\sigma} \odot \boldsymbol{\epsilon}, \quad \boldsymbol{\epsilon} \sim \mathcal{N}(0, \boldsymbol{I})
$$  
</div>


얻은  $Z$ 값을  $\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\sigma}^2)$  pdf에 대입하여  $\log Q(Z \mid X)$ 의 밀도값을 계산한다.

이때  $Q(Z \mid X)$ 도 가우시안이고,  $P(Z)$ 도 가우시안이기 때문에, 두 분포 간의 KL 발산  $D\_{KL}(Q \parallel P)$ 는 적분할 필요 없이 두 분포의  $\boldsymbol{\mu}$ 와  $\boldsymbol{\sigma}$ 만으로 이루어진 닫현 형태로 계산된다.

 $P(X \mid Z)$ 는 우도(디코더)이다. 잠재 변수  $Z$ 를 입력으로 받아, 원래의 데이터  $X$ 를 생성했을법한 확률 분포의 매개변수를 출력하는 디코더를 통해 계산한다.

만약  $X$ 가 흑백 이미지(0 또는 1)라면, 디코더는 베르누이 분포의 확률값  $p$ 를 출력하고, 만약  $X$ 가 연속형 실수 이미지라면, 디코더는 가우시안 분포의 평균  $\boldsymbol{\mu}\_x$ 를 출력한다.

출력된 분포의 매개변수로 실제 정답 데이터  $X$ 가 이 추정된 매개변수를 모수로 가지는 분포에서 나올 로그 확률 밀도  $\log P(X \mid Z)$ 를 계산한다(이게 MSE, binary Cross-entropy와 동일하다).
</div>





<div class="obsidian-callout" markdown="1">
Z를 어떤 pdf에 대입한다는 것인가?
</div>





<div class="obsidian-callout" markdown="1">
분산에 로그를 씌운 이유는?
</div>



## Mode-seeking과 GAN의 Mode Collapse의 유사성

변분 추론에서 나타나는 Reverse KL의 mode-seeking 성질은 GAN(Generative Adversarial Network)의 고질적인 문제인 mode collapse와 기하학적으로 같다.  

이 둘은 서로 다른 최적화 환경에서 발현되었을 뿐이다. Reverse KL에서  $Q$ 가  $P=0$ 인 구역을 덮으면 오버플로우가 발생하기 때문에 안전하고 확실한 피크(mode) 하나에만 확률 질량을 몰아넣게 된다.  

GAN 역시 최적화 목적 함수 자체는 대칭적일지라도, 실제 생성자(Generator)가 학습될 때의 그래디언트 흐름은 Reverse KL과 매우 유사하게 작동한다. 생성자  $G$ 가 실제 데이터 매니폴드  $P\_{data}$ 에 속하지 않은 허허벌판(가짜 데이터)을 생성하면 판별자(Discriminator)에게 가혹한 페널티를 받는다. 이 징벌을 피하기 위해 생성자는 판별자가 절대 가짜라고 구별할 수 없는, 가장 안전하고 확실한 국소적 지점(특정 mode, 예: 1가지의 완벽한 얼굴 형태) 한 곳으로만 모든 출력을 몰아버린다. 이것이 바로 다양성을 잃어버리는 mode collapse 현상이다.
