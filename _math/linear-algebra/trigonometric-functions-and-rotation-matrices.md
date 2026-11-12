---
layout: sidebar
title: 삼각함수와 회전행렬
collection_name: notes
nav_order: 1999
---

삼각함수의 정의  
삼각함수는 각의 크기를 변수로 하여 삼각비를 함수로 나타낸 것이다. 좌표평면 위에서 원점  $O$ 를 중심으로 하고 반지름의 길이가  $r$ 인 원을 그렸을 때,  $x$ 축의 양의 방향(시초선)과 동경  $OP$ 가 이루는 각의 크기를  $\theta$ 라 하자. 이때 원 위의 점  $P$ 의 좌표를  $(x, y)$ 라 하면, 사인(sine), 코사인(cosine), 탄젠트(tangent)는 다음과 같이 정의된다.  

<div class="math-container">
$$  
\sin \theta = \frac{y}{r}  
$$
</div>



<div class="math-container">
$$  
\cos \theta = \frac{x}{r}  
$$
</div>



<div class="math-container">
$$  
\tan \theta = \frac{y}{x}  
$$
</div>


이 정의를 통해 좌표  $(x, y)$ 는 반지름  $r$ 과 각  $\theta$ 를 이용해 ** $x = r \cos \theta$ ,  $y = r \sin \theta$ ** 로 표현할 수 있다. 이는 직교 좌표계를 극좌표계로 변환하거나 회전 변환을 유도하는 기초가 된다.  

삼각함수의 덧셈정리  
삼각함수의 덧셈정리는 두 각의 합이나 차에 대한 삼각함수의 값을 각의 삼각함수 값들로 표현하는 항등식이다. 이는 회전변환행렬을 유도할 때, 회전된 좌표의 위치를 계산하는 결정적인 도구가 된다. 사인과 코사인의 덧셈정리는 다음과 같다.  

<div class="math-container">
$$  
\sin(\alpha \pm \beta) = \sin\alpha \cos\beta \pm \cos\alpha \sin\beta  
$$
</div>



<div class="math-container">
$$  
\cos(\alpha \pm \beta) = \cos\alpha \cos\beta \mp \sin\alpha \sin\beta  
$$
</div>


이 공식은 기하학적으로 증명할 수 있으며, 특히 코사인의 덧셈정리인  $\cos(\alpha + \beta) = \cos\alpha \cos\beta - \sin\alpha \sin\beta$ 는 회전 변환 후의  $x$ 좌표를 구하는 데 직접적으로 사용된다.  

회전변환행렬 (Rotation Matrix)  
회전변환행렬은 선형 변환의 일종으로, 임의의 벡터를 원점을 중심으로 회전시키는 행렬이다. 2차원 유클리드 공간에서 원점을 중심으로 반시계 방향으로  $\theta$ 만큼 회전하는 변환을 행렬로 나타내는 방법은 크게 두 가지 관점으로 설명할 수 있다.  

첫째, 삼각함수의 덧셈정리를 이용한 대수적 접근이다. 점  $P(x, y)$ 를 원점 기준으로 거리  $r$ , 각도  $\alpha$ 라고 하면  $x=r\cos\alpha, y=r\sin\alpha$ 이다. 이 점을  $\theta$ 만큼 회전시킨 점  $P'(x', y')$ 은 거리  $r$ 은 유지되나 각도가  $\alpha+\theta$ 가 된다.  

<div class="math-container">
$$  
x' = r\cos(\alpha+\theta) = r(\cos\alpha\cos\theta - \sin\alpha\sin\theta) = (r\cos\alpha)\cos\theta - (r\sin\alpha)\sin\theta = x\cos\theta - y\sin\theta  
$$
</div>



<div class="math-container">
$$  
y' = r\sin(\alpha+\theta) = r(\sin\alpha\cos\theta + \cos\alpha\sin\theta) = (r\sin\alpha)\cos\theta + (r\cos\alpha)\sin\theta = y\cos\theta + x\sin\theta  
$$
</div>


둘째, 기저 벡터의 이동을 이용한 선형대수학적 접근이다. 표준 기저 벡터  $\mathbf{e}\_1 = \begin{bmatrix} 1 \ 0 \end{bmatrix}$ 은 회전 후 단위 원 위의 점  $(\cos\theta, \sin\theta)$ 로 이동한다.  $\mathbf{e}\_2 = \begin{bmatrix} 0 \ 1 \end{bmatrix}$ 은 회전 후  $(\cos(90^{\circ}+\theta), \sin(90^{\circ}+\theta))$  즉,  $(-\sin\theta, \cos\theta)$ 로 이동한다.  
두 가지 방법 모두 동일한 결과를 보이며, 이를 행렬로 표현하면 다음과 같다.  

<div class="math-container">
$$  
R_\theta = \begin{bmatrix} \cos \theta & -\sin \theta \\ \sin \theta & \cos \theta \end{bmatrix}  
$$
</div>


따라서 벡터  $\mathbf{x}$ 를  $\theta$ 만큼 회전시킨 벡터  $\mathbf{x}'$ 은 행렬 곱셈  $R\_\theta \mathbf{x} = \mathbf{x}'$ 을 통해 구할 수 있다.
