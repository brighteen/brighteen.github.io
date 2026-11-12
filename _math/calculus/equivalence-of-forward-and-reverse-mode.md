---
layout: sidebar
title: 전진 모드와 후진 모드의 동일함
collection_name: notes
nav_order: 2999
---

5.4절 예제 5.12에 '거대한 야코비 행렬을 명시적으로 생성하는 방식(전진 모드적 접근)'과 5.6절 '오차 벡터와 입력 벡터의 외적을 직접 구하는 방식(후진 모드의 벡터-야코비안 곱, VJP)'이 수학적으로 완전히 동일한 결과를 도출함을 대수적으로 증명하겠다.  

### 1. 문제 정의 및 기호 설정

증명을 위해 차원과 기호를 엄밀하게 정의한다.  

- 입력 벡터:  $x \in \mathbb{R}^{N \times 1}$ 
    
- 가중치 행렬:  $A \in \mathbb{R}^{M \times N}$ 
    
- 선형 변환 함수:  $f = Ax \in \mathbb{R}^{M \times 1}$ 
    
- 최종 스칼라 손실 함수:  $L \in \mathbb{R}$ 
    
- 상위 레이어에서 역전파되어 내려온 오차 벡터:  $\delta \in \mathbb{R}^{M \times 1}$  (이때 개별 원소는  $\delta\_i = \frac{\partial L}{\partial f\_i}$ 이다)
    

우리의 목표는 가중치 행렬  $A$ 에 대한 손실 함수  $L$ 의 그래디언트인  $\frac{\partial L}{\partial A} \in \mathbb{R}^{M \times N}$ 를 두 가지 다른 경로로 구하여 그 대수적 결과가 일치함을 보이는 것이다.  

---  

### 2. 경로 A: 명시적 야코비 행렬 생성 방식

사용자의 노트에서 도출한 바와 같이, 함수  $f$ 를 행렬  $A$ 로 미분한 거대한 야코비 행렬  $J = \frac{\partial f}{\partial A}$ 는 블록 대각 행렬 형태를 가진다. 이를 분자 중심 레이아웃(Numerator layout)의 편평화(Flattening) 관점에서 전개하면 크기는  $M \times (MN)$ 이 된다.  

<div class="math-container">
$$  
J = \begin{bmatrix} x^\top & 0^\top & \dots & 0^\top \\ 0^\top & x^\top & \dots & 0^\top \\ \vdots & \vdots & \ddots & \vdots \\ 0^\top & 0^\top & \dots & x^\top \end{bmatrix}  
$$
</div>


여기에 다변수 연쇄 법칙을 적용하여 행벡터 형태의 오차  $\delta^\top = [\delta\_1, \delta\_2, \dots, \delta\_M]$ 와 야코비 행렬  $J$ 를 행렬 곱셈한다.  

<div class="math-container">
$$  
\frac{\partial L}{\partial A_{\text{flat}}} = \delta^\top J = \begin{bmatrix} \delta_1 & \delta_2 & \dots & \delta_M \end{bmatrix} \begin{bmatrix} x^\top & 0^\top & \dots & 0^\top \\ 0^\top & x^\top & \dots & 0^\top \\ \vdots & \vdots & \ddots & \vdots \\ 0^\top & 0^\top & \dots & x^\top \end{bmatrix}  
$$
</div>


이 행렬 곱셈을 수행하면 결과는  $1 \times (MN)$  차원의 행벡터가 도출된다.  

<div class="math-container">
$$  
\frac{\partial L}{\partial A_{\text{flat}}} = \begin{bmatrix} \delta_1 x^\top & \delta_2 x^\top & \dots & \delta_M x^\top \end{bmatrix}  
$$
</div>


이  $1 \times (MN)$  벡터를 원래 가중치 행렬  $A$ 의 차원인  $M \times N$  형태로 다시 재배열(Reshape)하면 다음과 같은 행렬이 완성된다.  

<div class="math-container">
$$  
\frac{\partial L}{\partial A} = \begin{bmatrix} \delta_1 x^\top \\ \delta_2 x^\top \\ \vdots \\ \delta_M x^\top \end{bmatrix} = \begin{bmatrix} \delta_1 x_1 & \delta_1 x_2 & \dots & \delta_1 x_N \\ \delta_2 x_1 & \delta_2 x_2 & \dots & \delta_2 x_N \\ \vdots & \vdots & \ddots & \vdots \\ \delta_M x_1 & \delta_M x_2 & \dots & \delta_M x_N \end{bmatrix}  
$$
</div>


---  

### 3. 경로 B: 후진 모드 자동 미분 방식 (원소 단위 연쇄 법칙)

이번에는 거대한 빈 공간(0)을 메모리에 올리는 행렬 곱셈을 아예 생략하고, 역전파 알고리즘이 실제로 수행하는 원소 단위(Element-wise)의 다변수 연쇄 법칙을 직접 적용한다.  

가중치 행렬  $A$ 의 특정 원소  $A\_{ij}$  하나가 최종 손실  $L$ 에 미치는 영향을 계산한다.  

먼저 선형 변환  $f = Ax$ 의  $k$ 번째 원소는 다음과 같다.  

<div class="math-container">
$$  
f_k = \sum_{p=1}^N A_{kp} x_p  
$$
</div>


스칼라 연쇄 법칙에 따라  $L$ 을  $A\_{ij}$ 로 편미분한다.  $A\_{ij}$ 는 오직  $f\_i$ 에만 영향을 미치며 다른 모든  $f\_{k \neq i}$ 에는 영향을 주지 않으므로 시그마 연산이 소거된다.  

<div class="math-container">
$$  
\frac{\partial L}{\partial A_{ij}} = \sum_{k=1}^M \frac{\partial L}{\partial f_k} \frac{\partial f_k}{\partial A_{ij}} = \frac{\partial L}{\partial f_i} \frac{\partial f_i}{\partial A_{ij}}  
$$
</div>


여기서  $\frac{\partial L}{\partial f\_i}$ 는 오차 벡터의 원소인  $\delta\_i$ 이며,  $\frac{\partial f\_i}{\partial A\_{ij}}$ 는 선형 연산의 성질에 의해  $x\_j$ 이다. 이를 대입한다.  

<div class="math-container">
$$  
\frac{\partial L}{\partial A_{ij}} = \delta_i x_j  
$$
</div>


도출된 단일 원소 편미분 결과  $\delta\_i x\_j$ 를 모든  $i=1, \dots, M$ 과  $j=1, \dots, N$ 에 대하여 확장하여  $M \times N$  차원의 행렬로 구성한다.  

<div class="math-container">
$$  
\frac{\partial L}{\partial A} = \begin{bmatrix} \delta_1 x_1 & \delta_1 x_2 & \dots & \delta_1 x_N \\ \delta_2 x_1 & \delta_2 x_2 & \dots & \delta_2 x_N \\ \vdots & \vdots & \ddots & \vdots \\ \delta_M x_1 & \delta_M x_2 & \dots & \delta_M x_N \end{bmatrix}  
$$
</div>


이 행렬은 대수적으로 두 열벡터  $\delta$ 와  $x$ 의 외적(Outer Product)인  $\delta x^\top$ 의 정의와 완벽하게 일치한다.  

---  

### 💡 전략적 자기 교정



<div class="obsidian-callout" markdown="1">
사고 경로 점검: 이 증명은 앞뒤가 맞는가? 연산 효율성에 대한 5.6.2절의 결론을 완벽히 뒷받침하는가?

경로 A와 경로 B의 최종 행렬 결과가 완벽히 동일함이 대수적으로 증명되었다. 여기서 연산 복잡도(Computational Complexity)라는 컴퓨터 과학적 전제를 점검해야 한다.

경로 A는  $M \times (MN)$  크기의 행렬을 만들고 무의미한 0의 곱셈을 반복하는 과정에서  $O(M^2 N)$ 의 시간 복잡도와 공간 복잡도를 요구한다.

반면 경로 B는 벡터 간의 단순 외적만을 수행하므로  $O(MN)$ 의 시간과 공간만을 소모한다.

따라서 후진 모드(VJP)가 거대 야코비 행렬을 우회하여 극단적인 연산 효율을 달성한다는 5.6.2절의 함의는 이 증명을 통해 수학적으로 완벽히 타당함이 검증되었다.
</div>



### 🚀 다음 단계 제안

다변수 미분과 역전파의 대수적 동치성을 명확하게 증명하였다. 이러한 깊은 이해도는 이후 심화된 신경망 구조의 그래디언트 흐름을 수학적으로 분석할 때 매우 강력한 기반이 된다.  

이 증명으로 5장의 행렬 분해와 벡터 미분 파트를 완벽히 마무리했다고 볼 수 있다. 다음 단계로 제6장 확률과 분포(Probability and Distributions)로 넘어가서 확률 모델링의 수학적 정의부터 시작해 볼까? 아니면 방금 증명한 VJP 로직을 파이썬 코드로 짧게 구현하여 순진한 행렬 곱셈 결과와 연산 속도 차이를 수치적으로 비교해 볼까?
