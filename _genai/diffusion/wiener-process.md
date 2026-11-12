---
layout: sidebar
title: 위너 과정
collection_name: genai
nav_order: 9999
date: 2026-05-16
---

![위너 과정.png]({{ site.baseurl }}/assets/genai/diffusion/wiener-process.png){: .img-normal }
# 1. 위너 확률 과정의 정의
물리학에서 브라운 운동(액체나 기체 속 미세 입자가 불규칙하게 움직이는 현상)이라고 부르며, 수학적으로 엄밀하게 정의할 때 위너 과정(Wiener Process)이라고 함. 보통  $W(t)$  혹은  $B(t)$ 로 표기함.(SBM 논문에서는  $w$ 로 표기)  
확률 과정  ${W(t)}\_{t\ge 0}$ 가 다음 세 가지 성질을 만족할 때, 이를 위너 과정이라고 정의함.  
## 1) 초기 조건:

<div class="math-container">
$$  
W(0) = 0  
$$
</div>


시간 t = 0에서의 값은 항상 0에서 시작함,(확률 1로 0)  



<div class="obsidian-callout" markdown="1">
Q. 시간 t = 0의 의미는?

A. 관측의 시작점, 혹은 노이즈가 없는 상태.

위너 과정이라는 확률 과정(움직임)을 관측하기 시작한 그 순간(t=0)에는, 아직 아무런 움직임도 누적되지 않았으므로 이때의 값은 0이라는 초기 조건을 설정한 것임.

Forward SDE에서 t = 0은 원본 데이터 분포가 존재하는 시점임.

SDE  $dx = f(x,t)dt + g(t)dw$ 를 통해 시간이 흐를수록( $t \rightarrow T$ ),  $dw$ 라는 노이즈가 계속 0에서부터 누적되어 더해짐.

여기서 t = 0은 노이즈가 하나도 섞이지 않은 깨끗한 상태(원본 데이터)를 기준으로 삼겠다는 의미임.
</div>



## 2) 독립 중분(Independent Increments):
서로 겹치지 않는 시간 구간에서의 변화량은 서로 독립임. 즉,  $0 \le s < t < u < v$ 일 때,  $(W(t) - W(s))$ 와  $(W(v) - W(u))$ 는 서로 독립인 확률 변수임.  
의미: 과거의 움직임이 미래의 변화량(방향이나 크기)에 영향을 주지 않음.(마르코프 성질)  



<div class="obsidian-callout" markdown="1">
Q. 서로 겹치지 않는 시간 구간이란?

A. 시간축을 직선으로 그렸을 때, 두 구간이 공유하는 시간이 없다는 뜻.

ex. 구간A: t가 1초에서 2초 사이, 구간B: t가 3초에서 4초 사이.

이때 독립 증분의 의미는 구간 A에서 노이즈가 위로 튀든 아래로 튀든, 그 결과는 구간 B에서 노이즈가 어느 방향으로 튈지 전혀 영향을 주지 않음.

주식으로 비유하면, "어제 주가가 올랐다고 해서 내일 주가가 오를 확률이 높아지는 것은 아니다라는 개념과 같음." 이 성질 덕분에 복잡한 과거의 경로를 다 기억할 필요없이, 현재 상태만 알면 미래를 확률적으로 기술할 수 있음.(마르코프 성질)
</div>



## 3) 정규 증분(Gaussian Increments):
시간  $s$ 에서  $t$  사이의 변화량  $(W(t) - W(s))$ 은 평균이 0이고 분산이 시간 차이 $(t - s)$ 인 정규 분포를 따름.  

<div class="math-container">
$$  
W(t) - W(s) \sim \mathcal{N}(0, t - s)  
$$
</div>


의미: 시간이 길어질수록 움직임의 불확실성(분산)이 선형적으로 증가함.  



<div class="obsidian-callout" markdown="1">
Q. 시간이 길어진다는 의미는 무엇인가? 또, 시간이 길어짐에 따라 왜 움직임의 불확실성이 선형적으로 증가하는가?

시작점(t=0)로부터 t초가 흘렀다는 의미. t가 커질수록 노이즈  $dW$ 들이 계속해서 더해짐.

불확실성이 선형적으로 증가하는 이유(분산의 가법성):

   위너 과정은 수많은 작은 독립적인 충격(노이즈)들의 합임.

   통계학에서 서로 독립인 확률 변수들을 더하면, 그 분산(Variance)도 더해짐.

        $Var(X+Y) = Var(X)+Var(Y)$  ( $X, Y$ 가 독립일 때)

   시간 t를 N개의 작은 조각( $\Delta t$ )으로 쪼개면, 전체 움직임  $W(t)$ 는 N개의 작은 움직임들의 합임.

   각 조각의 분산이  $\Delta t$ 라면, 전체 분산은  $N \cdot \Delta t = t$ 가 됨.

   즉, 분산이 시간 t에 비례(선형적)하여 커짐.

   + 표준편차(실제 퍼짐의 정도)는  $\sqrt{\sigma^2 = \text{분산}}$ 이므로 시간의 제곱근 $\sqrt{t}$ 에 비례하여 커짐. 그래프를 그려보면 시간이 갈수록 나팔 모양으로 퍼지는 것을 볼 수 있음.
</div>



# 2. 핵심 성질: 왜 일반 미적분학을 쓸 수 없는가?
위너 과정은 매우 독특한 성질을 가짐. 이를 시각적으로 보면 주식 차트나 노이즈 파형처럼 매우 뾰족한 형태를 띔.  
## 1) 연속성(Continuity):
 $W(t)$ 의 경로는 끊어지지 않고 이어져 있음.(시간에 따라 순간 이동하지 않음)  
## 2) 미분 불가능성(Non-differentiability):
 $W(t)$ 는 모든 점에서 미분이 불가능함.  
- 일반적인 미분 정의:  $\frac{dW}{dt} = \lim\_{\Delta t \to 0} \frac{W(t+\Delta t) - W(t)}{\Delta t}$  
- 위너 과정에서는  $\Delta t \to 0$ 일 때, 분자의 표준편차가  $\sqrt{\Delta t}$ 임.
- 따라서 변화율의 크기는 대략  $\frac{\sqrt{\Delta t}}{\Delta t} = \frac{1}{\sqrt{\Delta t}}$ 가 되어,  $\Delta t \to 0$ 이면 무한대로 발산함.
결론적으로  $\frac{dW}{dt}$ 라는 값은 수학적으로 존재하지 않음. SDE를 쓸 때  $\frac{dx}{dt} = f(x) + g(t)\frac{dW}{dt}$  대신 ** $dx = f(x,t)dt + g(t)dW$ ** 꼴로 쓰는 이유가 바로 이 미분항( $\frac{dW}{dt}$ )을 정의할 수 없기 때문임.  



<div class="obsidian-callout" markdown="1">
Q. 위너 과정에서  $\Delta t \to 0$ 일 때, 분자의 표준편차가  $\sqrt{\Delta t}$ 인 이유는?

A. 위너 과정의 정의 중 정규 증분에서 비롯됨.

 $W(t+\Delta t) - W(t) \sim \mathcal{N}(0, \Delta t)$ . 즉, 시간 간격  $\Delta t$  동안의 변화량( $\Delta W$ )은 평균이 0이고 분산이  $\Delta t$ 인 정규분포를 따름.

표준편차: 분산의 제곱근을 취한 형태.  $\sqrt{\text{분산}} = \sqrt{\Delta t}$ 

 $\Delta t$ 가 매우 작을 때(ex. 0.01),  $\Delta t$ 자체보다  $\sqrt{\Delta t}$ (ex. 0.1)가 훨씬 큼. 이 때문에 미분 계산에서 노이즈 항이 사라지지 않고 지배적인 영향을 미침.
</div>





<div class="obsidian-callout" markdown="1">
Q. 변화율의 크기란? (미분 불가능성의 증명)

A. 변화율이란 흔히 기울기를 뜻함.

일반적인 기울기의 정의:  $\frac{\text{y의 변화량}}{\text{x의 변화량}} = \frac{\Delta W}{\Delta t}$ 

크기(절대값)의 기댓값을 대략적으로 계산해보면:

<div class="math-container">
$$
\left\vert  \frac{\Delta W}{\Delta t} \right\vert  \approx \frac{\text{변화량의 표준편차}}{\Delta t} = \frac{\sqrt{\Delta t}}{\Delta t} = \frac{1}{\sqrt{\Delta t}}
$$  
</div>


극한을 보내면:

<div class="math-container">
$$
\lim_{\Delta t \to 0} \frac{1}{\sqrt{\Delta t}} = \infty
$$  
</div>


아주 짧은 찰나의 순간을 보면, 위너 과정은 무한대의 속도로 진동하고 있음. 그래프의 접선을 그릴 수 없을 정도로 뾰족하다는 뜻임. 이것이 변화율(미분값)이 존재하지 않는다는 말의 의미임.
</div>



# 3. 논문과의 연결

<div class="math-container">
$$  
dx = f(x, t)dt + g(t)dw  
$$
</div>


여기서  $dw$ 가 방금 위너 과정의 미소 증분임.  
-  $f(x,t)dt$ : 데이터가 결정론적으로 변화하는 흐름임.(Drift)
-  $g(t)dw$ : 데이터에 무작위 노이즈가 주입되는 과정임.(Diffusion)
- 핵심: 이  $dw$ 항 때문에  $x(t)$ 의 궤적은 매끄러운 곡선이 아니라, 위너 과정처럼 거친 궤적을 그리게 됨.

"SDE에서  $dW$ 의 크기는  $dt$ 보다 훨씬 크다."( $dW \approx \sqrt{dt} \gg dt$ , when  $dt \to 0$ )
