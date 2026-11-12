---
layout: sidebar
title: 오차 벡터와 손실 함수의 기하학적 의미
collection_name: notes
nav_order: 4999
---

## 오차의 정의
통계학이나 머신러닝에서 단일 관치에 대한 오차  $e$ 는 실제값  $y$ 와 예측값  $\hat{y}$ 의 차이로 정의된다.  

<div class="math-container">
$$  
e = y - \hat{y}  
$$
</div>


관측치가  $n$ 개인 데이터셋의 경우, 독립적인  $n$ 개의 실제값과 예측값은 각각  $n$ 차원 벡터 공간 내의 단일 벡터  $\mathbf{y}$ 와  $\mathbf{\hat{y}}$ 로 정의된다. 이에 따라 오차 역시  $n$ 차원 공간의 오차 벡터  $\mathbf{e}$ 로 표현된다.  

<div class="math-container">
$$  
\mathbf{e} = \mathbf{y} - \mathbf{\hat{y}}  
$$
</div>


기하학적으로 거리(distance)는 두 점 사이의 간격을 측정하는 함수로 정의된다. 유클리드 벡터 공간에서 벡터  $\mathbf{y}$ 와  $\mathbf{\hat{y}}$  사이의 거리는 두 벡터의 차이에 대한 노름(norm)이다.  
 $L\_2$  노름(유클리드 거리)을 사용할 경우, 두 벡터 사이의 거리  $d$ 는 다음과 같다.  

<div class="math-container">
$$  
d(\mathbf{y}, \mathbf{\hat{y}}) = \Vert \mathbf{y} - \mathbf{\hat{y}}\Vert _2 = \sqrt{\sum_{i=1}^{n} (y_i - \hat{y}_i)^2}  
$$
</div>


이는 오차 벡터  $\mathbf{e}$ 의 크기  $\Vert \mathbf{e}\Vert \_2$ 를 구하는 것과 동일하다. 즉, 예측 벡터가 실제 벡터로부터 공간상에서 얼마나 떨어져 있는지를 측정하는 물리적/기하학적 길이가 오차의 총량을 나타낸다.  

## 오차 벡터의 스칼라화 및 손실 함수
기계학습 모델의 최적화를 수행하기 위해서는 모델의 현재 상태를 평가할 단일 스칼라 값이 요구된다. 오차 벡터  $\mathbf{e}$  자체는 방향과 크기를 동시에 갖는 다차원 벡터이므로, 이를 최적화 알고리즘의 목적 함수(Objective Function)로 직접 사용할 수 없다.  

따라서 오차 벡터의 기하학적 길이(거리)를 제곱하거나 평균을 내는 텐서 축소(Tensor Reduction) 연산을 통해 단일 스칼라 값인 손실(Loss)로 매핑한다. 평균 제곱 오차(MSE)의 구조는 다음과 같다.  

<div class="math-container">
$$  
Loss = \frac{1}{n} \Vert \mathbf{e}\Vert _2^2 = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2  
$$
</div>


정답 벡터와 예측 벡터의 차이가 손실이 되는 과정은, 다차원 오차 벡터의 크기를 기하학적 거리 개념을 차용하여 최적화 연산이 가능한 1차원 스칼라 상태로 압축하는 수학적 변환이다.  
또한 모든 관측 데이터  $n$ 개에 대한 각 오차의 크기가 손실 함수에 미치는 영향력이 동일하다고 가정한 것이다.  

## 다차원 목적 함수의 최적화 한계
목적함수의 산출값이 단일 스칼라가 아닌 벡터나 행렬 형태일 경우, 이를 단일 기준으로 최소화(또는 최대화)하는 일반적인 최적화 알고리즘에 직접 적용할 수 없다. 그 이유는 '대소 비교의 불가능성'과 미분 기하학적 '갱신 방향의 다의성' 때문이다.  
최적화는 특정 함수 상태가 다른 상태보다 '더 작냐 크냐'를 보는 과정이다. 실수체  $\mathbb{R}$ 의 원소인 스칼라는 임의의 두 원소 간에 대소 비교가 가능한 전순서 집합(totally ordered set, 임의의 두 원소를 비교할 수 있는 부분 순서 집합)이다. 반면, 2차원 이상의 벡터 공간  $\mathbb{R}^m$ 은 전순서를 가지지 않는다.  
목적함수  $\mathbf{F}(\mathbf{x})$ 가 다차원 벡터를 반환한다고 가정할 때, 두 산출 벡터  $\mathbf{v}\_1 = [1, 5]^T$ 와  $\mathbf{v}\_2 = [5, 1]^T$ 중 **어느 것이 더 좋은가**를 결정할 기준이 없다.  
해를 찾을 때(보통 근사) 주로 쓰이는 경사하강법은 파라미터  $\mathbf{w}$ 에 대한 목적함수의 1차 미분값을 계산하여, 함수값이 감소하는 방향으로 파라미터를 갱신한다. 목적함수  $L$ 이 스칼라 함수인 경우,  $L: \mathbb{R}^n \rightarrow \mathbb{R}$ 의 1차 미분은 그래디언트(Gradient)  $\nabla L$ 이다. 이는 파라미터 공간에서 단일한 상승 방향 벡터를 형성한다. 어떤 최소화 문제의 경우, 음의 그래디언트를 구해 기울기가 가파른 반대 방향으로 이동한다.  

<div class="math-container">
$$  
\mathbf{w}_{t+1} = \mathbf{w}_t - \eta \nabla L(\mathbf{w}_t)  
$$
</div>


그러나 목적함수가 벡터 함수  $\mathbf{F}: \mathbb{R}^n \rightarrow \mathbb{R}^m$  ( $m > 1$ )인 경우, 1차 미분은 다차원 변환 행렬인 야코비 행렬(Jacobian Matrix)  $\mathbf{J}$ 가 된다. 야코비 행렬은  $m$ 개의 각기 다른 산출 성분이  $n$ 개의 파라미터에 대해 갖는 변화율을 행렬 형태로 나타낸다. 결과적으로  $m$ 개의 서로 다른 갱신 방향이 도출되며, 이 행렬을 단일 파라미터 벡터  $\mathbf{w}$ 에서 직접 감산할 연산이 성립하지 않는다.  
만약 어떤 함수의 결과가 벡터인 시스템을 최적화하기 위해서는 벡터의 각 성분에 가중치 벡터 $\mathbf{w}$ 를 내적하여 차원을 1차원으로 축소하는 스칼라화(scalarization) 방식을 쓰기도 한다.  

<div class="math-container">
$$  
L_{scalar} = \sum_{i=1}^{m} w_i f_i(\mathbf{x})  
$$
</div>


## 최적화의 기하학적 의미
데이터 행렬  $\mathbf{X}$ 와 정답 벡터  $\mathbf{y}$ 가 주어졌을 때, 최적화의 목적은 예측 벡터  $\mathbf{\hat{y}} = \mathbf{Xw}$ 와 정답 벡터 사이의  $L\_2$  노름을 최소화하는 파라미터  $\mathbf{w}$ 를 찾는 것이다.  

<div class="math-container">
$$  
Loss = \Vert \mathbf{y} - \mathbf{Xw}\Vert _2^2  
$$
</div>


이는  $n$ 차원 공간에서 점  $\mathbf{y}$ 와  $\mathbf{X}$ 의 열벡터들이 생성하는 부분 공간(Subspace) 내의 한 점  $\mathbf{\hat{y}}$  사이의 거리를 최소화 하는 문제이다.  
임의의 부분 공간  $V$  위에 있지 않은 한 점  $\mathbf{y}$ 에서  $V$ 에 도달하는 최단 거리는  $\mathbf{y}$ 에서  $V$ 에 내린 수선의 발까지의 거리([직교 투영]({{ site.baseurl }}/mml/Part I Mathematical Foundations/3. Analytic Geometry/3.8 Orthogonal Projections/))이다. 오차 벡터  $\mathbf{e} = \mathbf{y} - \mathbf{\hat{y}}$ 가 부분 공간  $V$  내의 모든 벡터와 수직을 이루어야 하며,  $\mathbf{e}$ 가  $V$ 의 기저인  $\mathbf{X}$ 의 모든 열벡터와 직교할 때, 거리는 최소가 된다.  

오차 벡터가  $\mathbf{X}$ 의 열공간과 직교해야 한다는 조건은 다음 방정식을 만족하는 것과 같다.  

<div class="math-container">
$$  
\mathbf{X}^T \mathbf{e} = \mathbf{0}  
$$
</div>



<div class="math-container">
$$  
\mathbf{X}^T (\mathbf{y} - \mathbf{Xw}) = \mathbf{0}  
$$
</div>


이를 전개하면 해( $\mathbf{w}$ )를 구하는 정규 방정식이 도출된다.  

<div class="math-container">
$$  
\mathbf{X}^T \mathbf{Xw} = \mathbf{X}^T \mathbf{y}  
$$
</div>



<div class="math-container">
$$  
\mathbf{w} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}  
$$
</div>


여기서 행렬  $\mathbf{P} := \mathbf{X}(\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T$ 는 임의의 벡터를  $\mathbf{X}$ 의 열공간으로 사영하는 직교 투영 행렬이다. 즉, 최적화된 예측값  $\mathbf{\hat{y}}$ 는 정답 벡터  $\mathbf{y}$ 에 직교 투영 행렬을 곱한 결과와 동일하다.  

결론적으로 최적화는 수치적으로 손실 함수의 하강 방향을 찾는 과정이지만, 벡터 공간의 관점에서는 정답 벡터를 모델이 표현 가능한 부분 공간으로 직교 투영하여 오차 벡터의 길이를 최소화하는 기하학적 연산이다.  

Reference  
wikipedia
