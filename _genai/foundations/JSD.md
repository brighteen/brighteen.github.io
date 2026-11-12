---
layout: sidebar
title: JSD
collection_name: genai
nav_order: 6999
date: 2026-05-16
---

### 정보량과 엔트로피 (Entropy)
정보량이 생산되는 과정에서, 이를 표현하는 데 필요한 **최소 자원량(비트 수 등)의 기댓값**임.  
확률분포  $P$ 의 엔트로피는  $H(P) = E\_{x\sim P}[-\log P(x)]$ 로 정의됨.  

### Kullback-Leibler Divergence (KLD)
어떤 이상적인 확률분포  $P$ 가 있을 때, 이를 근사하는 확률분포  $Q$ 를 사용하여 데이터를 인코딩(샘플링)할 경우 발생하는 **엔트로피의 증가량(정보 손실량, 비효율성)** 을 의미.  

<div class="math-container">
$$  
D_{KL}(P\vert \vert Q) = H(P,Q) - H(P)  
$$
</div>


* 여기서  $H(P,Q)$ 는 크로스 엔트로피.
* 최소 자원량인  $H(P)$ 보다  $H(P,Q)$ 는 항상 크거나 같으므로, KLD는 **항상 0 이상**임 ( $D\_{KL} \ge 0$ ).
* **비대칭성 (Asymmetric):**  $D\_{KL}(P\vert \vert Q) \neq D\_{KL}(Q\vert \vert P)$ 이므로, 이는 엄밀한 의미의 '거리(Distance)' 개념이 아님.
* **한계:** 두 분포가 겹치지 않는 곳( $Q(x)=0$ 인 곳)에서는 값이 무한대로 발산할 수 있어 학습이 불안정해질 수 있음.

### Jensen-Shannon Divergence (JSD)
KLD의 비대칭성과 발산 문제를 해결하기 위해, 두 분포의 **평균 분포**  $M$  을 도입하여 정의한 척도임.  

<div class="math-container">
$$  
M = \frac{P+Q}{2}  
$$
</div>



<div class="math-container">
$$  
JSD(P\vert \vert Q) = \frac{1}{2}D_{KL}(P\vert \vert M) + \frac{1}{2}D_{KL}(Q\vert \vert M)  
$$
</div>


**JSD의 핵심 특징:**  
1.  **대칭성 (Symmetric):**  $P$ 와  $Q$ 를 바꿔도 값이 동일 ( $JSD(P\vert \vert Q) = JSD(Q\vert \vert P)$ ). 따라서 두 분포 사이의 **'거리' 개념**으로 해석하기 적합.
2.  **값의 범위 (Bounded):**  $0 \le JSD \le \log 2$ 로 값이 항상 제한되어 있어, KLD처럼 무한대로 발산하지 않음.  
3.  **안정성:** 평균 분포  $M$  덕분에 두 분포  $P, Q$ 가 서로 겹치지 않는 영역에서도 안정적으로 값을 계산할 수 있음. 이는 GAN 학습의 안정성에 중요한 역할을 함.
