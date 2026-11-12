---
layout: sidebar
title: 그래디언트가 항상 가장 가파른 방향을 가리키는 이유
collection_name: notes
nav_order: 2999
---

우리는 다차원 공간에서 임의의 방향 벡터  $\boldsymbol{v}$  (크기가  $1$  인 단위 벡터,  $\Vert \boldsymbol{v}\Vert = 1$ )로 이동했을 때 함수값  $f(\boldsymbol{x})$ 가 최소가 되길 원한다. 이때 방향 도함수는 기울기 벡터(그래디언트)와 방향 벡터의 내적으로 정의된다.  

<div class="math-container">
$$  
D_{\boldsymbol{v}} f(\boldsymbol{x}) = \nabla f(\boldsymbol{x}) \cdot \boldsymbol{v}  
$$
</div>


이때 그래디언트 벡터  $\nabla f(\boldsymbol{x})$ 가 어떤 방향을 가르키는지 확인해보자.  

위 변화율을 벡터 내적으로 다음과 같이 정의할 수 있다.  

<div class="math-container">
$$  
\nabla f(\boldsymbol{x}) \cdot \boldsymbol{v} = \Vert \nabla f(\boldsymbol{x})\Vert  \Vert \boldsymbol{v}\Vert  \cos(\theta)  
$$
</div>


(여기서  $\theta$  는 두 벡터 사이의 각도)  
 $\Vert \boldsymbol{v}\Vert = 1$  이고  $\Vert \nabla f(\boldsymbol{x})\Vert$  는 현재 위치에서 고정된 상수이므로,  

<div class="math-container">
$$  
\text{변화율} = \Vert \nabla f(\boldsymbol{x})\Vert  \cos(\theta)  
$$
</div>


함수값의 변화율을 가장 크게 만들기 위해서는  $\cos(\theta)$ 가 가질 수 있는 최댓값 1을 만드는 것이다. 즉, 두 벡터( $\nabla f(\boldsymbol{x}), \boldsymbol{v}$ )의 사잇각이  $0^\circ$ 이어야 한다.  
결론적으로, 함수값이 제일 커지는 방향( $\boldsymbol{v}$ )을 구해보니, 그 방향이 그래디언트( $\nabla f$ )의 방향과 일치한다는 것을 알 수 있다.  

일반적인 머신러닝의 목표는 이 함수값을 가장 크게 감소시키는 것, 즉 이 내적 값을 가장 작은 음수로 만드는 방향  $\boldsymbol{v}$  를 찾는 것이다.  
이 결과값을 최소화할 수 있는 유일한 변수는 두 벡터사이 각도  $\cos(\theta)$ 이며,  $\cos(\theta)$  가 가질 수 있는 최솟값은  $-1$  이기 때문에 이때 두 벡터의 사잇각은  $\theta = 180^\circ$ 가 된다.  
두 벡터의 각도가  $180^\circ$ 라는 것은, 우리가 이동해야 할 최적의 방향  $\boldsymbol{v}$  가 기울기 벡터  $\nabla f(\boldsymbol{x})$ 와 정반대 방향을 향해야 함을 뜻한다. 따라서 우리는 항상 '음의 기울기( $-\nabla f$ )'를 따라 내려가야 목적함수를 그 지점에서 가장 최소화할 수 있다.
