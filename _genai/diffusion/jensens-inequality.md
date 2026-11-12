---
layout: sidebar
title: 젠슨 부등식
collection_name: genai
nav_order: 9999
date: 2026-04-29
---

기댓값의 볼록 함수와 볼록 함수의 기댓값 사이에 성립하는 부등식이다.  



<div class="obsidian-callout" markdown="1">
먼저 볼록 함수에 대해 상기해보자.

어떤 함수  $f(x)$ 가 볼록 함수이려면 다음 3 가지 조건을 만족해야 한다(오목 함수는 부등호의 방향만 뒤집으면 성립한다).

1. 함수의 정의역 내에 있는 임의의 두 점( $x\_1, x\_2$ )을 연결한 직선이 항상 함수 곡선의 위쪽(또는 같은 위치)에 있어야 한다.

<div class="math-container">
$$
f(tx_1 + (1-t)x_2) \le tf(x_1) + (1-t)f(x_2), \quad t \in [0, 1]
$$  
</div>


2. 함수  $f(x)$ 가 미분 가능하다면, 함수 곡선 위의 임의의 점  $x\_0$ 에서 그은 접선이 항상 함수 곡선보다 아래쪽(또는 같은 위치)에 있어야 한다. 임의의 점  $x, x\_0$ 에 대하여 다음 부등식을 만족해야 한다.

<div class="math-container">
$$
f(x) \ge f(x_0) + \nabla f(x_0)^T (x - x_0)
$$  
</div>


3. 함수가 2번 미분 가능할 때 모든  $x$ 에 대해 2차 도함수가 0보다 크거나 같아야 한다.

<div class="math-container">
$$
f''(x) \ge 0
$$  
</div>


다변수인 경우 스칼라  $f''(x)$  대신 2차 편미분 행렬 헤시안 행렬( $\nabla^2 f(x)$ )이 양의 준정부호여야 한다. 즉, 임의의 영벡터가 아닌 벡터  $v$ 에 대해 다음이 성립해야 한다.

<div class="math-container">
$$
v^T \nabla^2 f(x) v \ge 0
$$  
</div>


최적화 관점에서 목적 함수가 볼록 함수라면 지역 최솟값이 전역 최솟값이므로, 그래디언트를 따라가면 최적해에 도달할 수 있다.
</div>



## 젠슨 부등식
확률 변수  $X$ 와 임의의 함수  $f$ 에 대하여, 젠슨 부등식은 함수  $f$ 가 볼록(Convex)한지 오목(Concave)한지에 따라 기댓값연산과 함께 함수의 적용 순서가 바뀌었을 때의 대소 관계를 정의한다.  

- 함수  $f$ 가 볼록 함수(Convex function)일 때:

<div class="math-container">
$$  
f(\mathbb{E}[X]) \le \mathbb{E}[f(X)]  
$$
</div>


(기댓값의 함수 값은 함수의 기댓값보다 작거나 같다)  

- 함수  $f$ 가 오목 함수(Concave function)일 때:

<div class="math-container">
$$  
f(\mathbb{E}[X]) \ge \mathbb{E}[f(X)]  
$$
</div>


(기댓값의 함수 값은 함수의 기댓값보다 크거나 같다)  

왜 이런 부등식이 성립하는가?  
이는 기하학적으로 할선의 성질에서 기인한다. 볼록 함수(예:  $f(x) = x^2$ )의 그래프 위에 있는 임의의 두 점( $x\_1, x\_2$ )을 직선으로 연결한 선분(할선)을 생각해보자. 이 선분은 두 점 사이 구간에서 항상 그래프보다 위쪽(또는 같은 위치)에 존재한다.  

<div class="math-container">
$$  
f(tx_1 + (1-t)x_2) \le tf(x_1) + (1-t)f(x_2), \quad t \in [0, 1]  
$$
</div>


기댓값은 확률을 가중치로 사용하는 무한한 점들의 가중 평균을 구하는 것이다. 따라서 이 두 점에 대한 볼록 함수의 기하학적 성질을 확률 분포 전체로 일반화한 것이 젠슨 부등식이다.  

젠슨 부등식은 최적화에서 목적함수가 intractable한 경우, 하한(lower bound)을 정의할 수 있게 한다.  

관측 데이터  $x$ 와 이 데이터를 생성하는 데 관여하는 숨겨진 잠재 변수  $z$ 가 존재할 때, 우리가 최대화하고 싶은 관측 데이터의 로그 우도  $\log p(x)$ 는 다음과 같이 marginalization을 통해 표현된다.  

<div class="math-container">
$$  
\log p(x) = \log \left( \int p(x\vert z)p(z) dz \right)  
$$
</div>


이떄 임의의 확률 밀도 함수  $q(z)$ 를 분모와 분자에 곱한 후 기댓값 형태로 바꿔준다.  

<div class="math-container">
$$  
\log p(x) = \log \left(q(z)\frac{p(x,z)}{q(z)} \right)  
$$
</div>



<div class="math-container">
$$  
= \log \left( \mathbb{E}_{z \sim q} \left[ \frac{p(x,z)}{q(z)} \right] \right)  
$$
</div>


로그 함수는 오목 함수이므로  

<div class="math-container">
$$  
\log p(x) \ge \mathbb{E}_{z \sim q} \left[ \log \frac{p(x,z)}{q(z)} \right]  
$$
</div>


이때 우변은 좌변의 하한이므로, 우변을 최대화하는 것은, 좌변의 하한을 최대화하는 것과 같다.
