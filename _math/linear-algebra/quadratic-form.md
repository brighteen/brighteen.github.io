---
layout: sidebar
title: 이차 형식
collection_name: notes
nav_order: 1999
---

수론과 선형대수학에서 이차 형식은 다변수 2차 동차다항식이다(위키백과는 친절하지 않다. 사실 가장 친절했을 수 있다).  

[양의 정부호]({{ site.baseurl }}/notes/linear-algebra/positive-definite/)  

이차 형식(quadratic form) 은 다차원 벡터  $\boldsymbol{v}$ 를 입력으로 받아 단 하나의 스칼라(실수) 값을 내뱉는 다변수 스칼라 함수  $f(\boldsymbol{v})$ 이다. 단순하게 2차원 공간  $\mathbb{R}^2$ 의 임의의 대칭 행렬  $\boldsymbol{A}$ 와 변수 벡터  $\boldsymbol{v}$ 를 다음과 같이 정의한다.  

<div class="math-container">
$$  
\boldsymbol{A} = \begin{bmatrix} a & b \\ b & c \end{bmatrix}, \quad \boldsymbol{v} = \begin{bmatrix} x \\ y \end{bmatrix}  
$$
</div>


이를 이차 형식  $\boldsymbol{v}^\top \boldsymbol{A} \boldsymbol{v}$ 으로 전개하면 다음과 같다.  

<div class="math-container">
$$  
\begin{bmatrix} x & y \end{bmatrix} \begin{bmatrix} a & b \\ b & c \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} x & y \end{bmatrix} \begin{bmatrix} ax + by \\ bx + cy \end{bmatrix} = ax^2 + 2bxy + cy^2  
$$
</div>


대수적으로 이차 형식은 변수들의 1차항(예:  $x$ ,  $y$ )이나 상수항 없이, 오직 2차항( $x^2, y^2, xy$ )들로만 구성된 순수한 이차 다항식을 행렬의 형태로 표기한 것이다.  



<div class="obsidian-callout" markdown="1">
 $\boldsymbol{A}$ 가 비대칭 행렬인 경우를 생각해보자.  $\boldsymbol{B} = \begin{bmatrix} 1 & 3 \\ 1 & 2 \end{bmatrix}$ 에 대해 이차 형식을 전개하면  $1x^2 + 4xy + 2y^2$ 가 되는데, 이 결과는 대칭 행렬  $\boldsymbol{A} = \begin{bmatrix} 1 & 2 \\ 2 & 2 \end{bmatrix}$ 를 전개했을 때와 동일하다. 이차 형식의 계수는 행렬의 대칭적인 성분( $\frac{\boldsymbol{B} + \boldsymbol{B}^\top}{2}$ )에 의해서만 결정되며 비대칭 성분은 내적 과정에서 서로 상쇄되어 사라진다.
</div>



 $\boldsymbol{v}^\top \boldsymbol{A} \boldsymbol{v}$ 을 다음과 같이 괄호로 묶어 표현하면 기하학적 의미를 생각해볼 수 있다.  

<div class="math-container">
$$  
\boldsymbol{v}^\top \boldsymbol{A} \boldsymbol{v} = \boldsymbol{v}^\top (\boldsymbol{A} \boldsymbol{v})  
$$
</div>


행렬  $\boldsymbol{A}$ 는 다차원 공간을 찌그러뜨리거나 회전시키는 선형 변환이다. 입력 벡터  $\boldsymbol{v}$ 에 행렬  $\boldsymbol{A}$ 를 곱하면, 원래의 위치에서 새로운 위치로 변환된 벡터  $\boldsymbol{A} \boldsymbol{v}$ 가 만들어진다.  
따라서 이는 원래의 벡터  $\boldsymbol{v}$ 와 변환된 벡터  $\boldsymbol{A} \boldsymbol{v}$  사이의 내적(Inner Product)이다. 원래 벡터와 변환된 벡터가 얼마나 같은 방향을 바라 보고 있는가(유사도)를 측정한다.  

결론적으로 이차 형식의 결과값(스칼라)의 부호와 크기는 다음을 의미한다.  
- 값이 양수( $> 0$ )일 때: 변환된 벡터  $\boldsymbol{A} \boldsymbol{v}$ 가 원래 벡터  $\boldsymbol{v}$ 와 예각(90도 미만)을 이룬다. 즉, 행렬  $\boldsymbol{A}$ 가 벡터를 이리저리 뒤틀었지만, 전반적인 방향성은 원래 벡터가 가리키던 앞쪽 공간으로 유지시켰음을 뜻한다. (이 값이 모든  $\boldsymbol{v}$ 에 대해 양수이면 행렬  $\boldsymbol{A}$ 를 '양의 정부호'라고 부른다.)
- 값이 0일 때: 변환된 벡터  $\boldsymbol{A} \boldsymbol{v}$ 가 원래 벡터  $\boldsymbol{v}$ 와 정확히 90도로 직교한다.
- 값이 음수( $< 0$ )일 때: 변환된 벡터  $\boldsymbol{A} \boldsymbol{v}$ 가 원래 벡터  $\boldsymbol{v}$ 와 둔각(90도 초과)을 이루어, 기하학적으로 뒤쪽 공간으로 변환되었음을 의미한다.

만약  $\boldsymbol{v}^\top \boldsymbol{A} \boldsymbol{v}$ 의 결과가 어떤 상수  $k$ 라고 한다면 어떨까.  

이차 형식이 가지는 기하학적 의미는 무엇인가?  


참고  
wikipedia
