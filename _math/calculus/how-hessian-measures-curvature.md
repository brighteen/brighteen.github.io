---
layout: sidebar
title: 헤시안 행렬이 어떻게 곡률을 '측정'하는가
collection_name: notes
nav_order: 2999
---

헤시안 행렬이 어떻게 곡률을 '측정'하는지는 행렬의 고유값을 통해 대수적으로 증명된다. 특정 좌표에서 구한 헤시안 행렬의 고유값  $\lambda$ 들의 부호를 분석하면, 그 좌표 주변 공간의 3차원 기하학적 형태를 확인할 수 있다.  

모든 고유값이 양수인 경우(Positive Definite), 모든 방향으로 아래로 볼록하한 포물면을 형성한다. 기하학적으로 이 지점이 극소점이다.  
모든 고유값이 음수인 경우(Negative Definite), 모든 방향으로 위로 볼록한 포물면을 형성한다. 이 지점이 극대점이다.  
고유값의 부호가 양수와 음수가 섞여 있는 경우(Indefinite), 어떤 방향으로는 아래로 볼록하지만, 직교하는 다른 방향으로는 위로 볼록한 기하학적 형태를 띈다. 이를 안장점(Saddle Point)이라고 부르며, 1차 미분이 0임에도 극소점이 아니다.  

간단한 예시로 함수  $f(x,y)=xy$ 를 생각해보자. 이 다변수 함수의 헤시안 행렬을 구하기 앞서, 1차 편도함수를 계산하면 다음과 같다.  

<div class="math-container">
$$  
\frac{\partial f}{\partial x} = y, \quad \frac{\partial f}{\partial y} = x  
$$
</div>


1차 편도함수들을 다시 다른 변수에 대해 편미분하여 2차 편도함수들을 구할 수 있다.   

<div class="math-container">
$$  
H = \begin{bmatrix} \frac{\partial^2 f}{\partial x^2} & \frac{\partial^2 f}{\partial x \partial y} \\ \frac{\partial^2 f}{\partial y \partial x} & \frac{\partial^2 f}{\partial y^2} \end{bmatrix} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}  
$$
</div>


(대각 성분이 0인 이유는 두 변수가 서로 종속적이기 때문인가?)  
(클레로의 정리에 의해 교차 편도함수가 동일함을 확인할 수 있다.)  
이 함수의 헤시안 행렬이 좌표  $(x,y)$ 에 의존하지 않는 상수행렬이다. 이는 곡면  $f(x,y)=xy$ 의 휘어지는 성질(곡률)이 공간상의 어느 지점에서나 동일하게 유지된다는 것을 의미한다.  

위 헤시안 행렬의 특성 방정식을 세워 고유값을 구하여 기하학적 의미를 파악해보자.  

<div class="math-container">
$$  
\det(H - \lambda I) = \det\left( \begin{bmatrix} -\lambda & 1 \\ 1 & -\lambda \end{bmatrix} \right) = (-\lambda)(-\lambda) - (1)(1) = \lambda^2 - 1 = 0  
$$
</div>


따라서 고유값은  $\lambda\_1 = 1, \quad \lambda\_2 = -1$ 이다.  
고유값의 부호가 양수와 음수가 혼재되어 있다. 이 경우 해당 지점은 극소점도 극대점도 아닌 안장점이 된다.  
고유값에 대응되는 고유벡터의 방향은 각각  $y=x, y=-x$ 이다.  
고유값이 하나는 양수, 하나는 음수라는 것은 이 곡면이 특정 방향으로는 아래로 볼록하게 휘어져 있고, 그에 직교하는 다른 방향으로는 위로 볼록하게 휘어져 있음을 뜻한다. 또한  $x$ 축이나  $y$ 축을 따라서만 이동하면  $f(x,0) = 0$ ,  $f(0,y) = 0$ 이 되어 고도가 변하지 않는다(곡률 0).  
반면, 고유벡터 방향인 대각선  $y=x$  방향을 따라가면  $f(x,x) = x^2$ 이 되어 아래로 볼록한 포물선( $\lambda\_1 = 1$ )이 되고,  $y=-x$  방향을 따라가면  $f(x,-x) = -x^2$ 이 되어 위로 볼록한 포물선( $\lambda\_2 = -1$ )이 된다.  
![헤시안 행렬의 기하학적 의미.png]({{ site.baseurl }}/assets/notes/calculus/geometric-meaning-of-hessian.png){: .img-normal width="300" }
결론적으로  $f(x,y) = xy$ 가 그리는 곡면은 전형적인 쌍곡포물면(Hyperbolic Paraboloid), Saddle 형태의 지형이다.
