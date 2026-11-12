---
layout: sidebar
title: 양의 정부호
collection_name: notes
nav_order: 1999
---

양의 정부호(positive definite) 행렬의 정의는 다음과 같다.  
영벡터가 아닌 임의의 벡터  $\mathbf{x}$ 와 대칭 행렬  $A$ 에 대해 다음이 성립한다면  $A$ 는 양의 정부호 행렬이다.  

<div class="math-container">
$$  
\mathbf{x}^TA\mathbf{x} \gt 0  
$$
</div>


부호는 양(+) 또는 음(-)의 성질을 가지는 수학의 개념이자 이를 나타내는 수학 기호이다. 양의 성질을 가지는 부호를 양부호로, 음의 성질을 가지는 부호를 음부호로 부른다.  

임의의 양의 실수  $a$ 와 임의의 실수  $b$ 가 있을 때,  $b$ 가 양수이건 음수이건  $a$ 를 곱했을 때 scale되지만 부호가 그대로 유지된다. 방향은 그대로인 것이다. 양의 정부호라는건 이렇게 부호를 유지해주는 성질을 가진다.  
이를 벡터에 적용시켜보자. 어떤 벡터의 방향을 뒤집지 않는 변환을 양의 정부호행렬이라한다.  
 $\mathbf{x}$ 에 변환  $A$ 를 적용시킨  $A\mathbf{x}$ 에 자기 자신을 내적한 것이 0보다 크다는 것은 변환  $A$ 가  $\mathbf{x}$ 의 방향을 뒤집거나 직교하게 만들지 않는다는 것이다.  

양의 정부호 행렬이 가지는 기하학적 의미를 생각해보자.  $\mathbf{x}^T A\mathbf{x}$ 는 벡터  $\mathbf{x}$ 에  $A$ 라는 변환을 취한 벡터  $A\mathbf{x}$ 에 자기 자신( $\mathbf{x}$ )를 transpose한  $\mathbf{x}^T$ 를 곱하는 연산이다. 이는  $\mathbf{x}$ 가 변환된 좌표계에서 자기 자신과 곱하는 과정인데 이 결과가 0보다 크다는 것은 원래 벡터  $\mathbf{x}$ 와 변환된 벡터  $A\mathbf{x}$  사이의 각도가 90도 미만(예각)이라는 뜻이며, 같은 말로 부호가 유지되었음을 의미한다.  
내적의 의미:  $x^\top (Ax) > 0$   $\iff$   $x \cdot Ax = \vert x\vert \vert Ax\vert \cos\theta > 0$   $\iff$   $\cos\theta > 0$  (즉,  $-90^\circ < \theta < 90^\circ$ )  
이를 만족하는 변환  $A$ 를 양의 정부호 행렬이라 한다.  

---  

MML [3.2절]({{ site.baseurl }}/mml/Part I Mathematical Foundations/3. Analytic Geometry/3.2 Inner Products/) 예제 3.3은 다음과 같다.  


<div class="obsidian-callout" markdown="1">
 $V = \mathbb{R}^2$ 라고 하자. 만약 우리가 다음과 같이 정의한다면:

<div class="math-container">
$$
\langle x, y \rangle := x_1 y_1 - (x_1 y_2 + x_2 y_1) + 2x_2 y_2 \quad (3.9)
$$  
</div>


그렇다면  $\langle \cdot, \cdot \rangle$ 은 내적이지만 점곱과는 다르다. 이에 대한 증명은 연습 문제로 남긴다.
</div>



이때 위 수식을 다시 쓰면 아래와 같다.  

<div class="math-container">
$$  
\langle x, y \rangle = \begin{bmatrix} x_1 & x_2 \end{bmatrix} \begin{bmatrix} 1 & -1 \\ -1 & 2 \end{bmatrix} \begin{bmatrix} y_1 \\ y_2 \end{bmatrix}  
$$
</div>


앞서 본 점곱(dot product)는 사실  $x^\top I y$ 꼴로 두 벡터 사이에 단위행렬을 곱하는 연산이며, 일반적인 내적(inner products)는 두 벡터 사이 행렬이 단위행렬이 아니다. 위 연산이 내적의 조건을 만족하는지 확인해보려면 자기 자신과 내적했을 때 양수가 나오는지 확인하면 된다.  

<div class="math-container">
$$  
\langle x, x \rangle = x_1^2 - 2x_1 x_2 + 2x_2^2  
$$
</div>


이를 완전제곱식으로 바꾸면,  

<div class="math-container">
$$  
= (x_1 - x_2)^2 + x_2^2  
$$
</div>


인데  $x\_1,x\_2$ 는 실수이기 때문에 두 항은 모두 0보다 크거나 같으므로 위 연산은 일반적인 내적이다.  

---  

## 양의 준정부호

그럼  $\begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix}$ 인 경우는 어떨까.  

<div class="math-container">
$$  
\begin{aligned} \mathbf{x}^\top A \mathbf{x} &= \begin{bmatrix} x_1 & x_2 \end{bmatrix} \begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \end{bmatrix} \\ &= \begin{bmatrix} x_1 & x_2 \end{bmatrix} \begin{bmatrix} x_1 - x_2 \\ -x_1 + x_2 \end{bmatrix} \\ &= x_1(x_1 - x_2) + x_2(-x_1 + x_2) \\ &= x_1^2 - x_1 x_2 - x_1 x_2 + x_2^2 \\ &= x_1^2 - 2x_1 x_2 + x_2^2 \end{aligned}  
$$
</div>


이 식을 완전제곱식으로 표현하면 다음과 같다.  

<div class="math-container">
$$  
= (x_1 - x_2)^2  
$$
</div>


만약  $\mathbf{x}$ 가  $\forall x\_i \ne 0, x\_1 = x\_2$ 라면(e.g.,  $\begin{bmatrix} 1 \\ 1 \end{bmatrix}$ ) 영벡터가 아님에도 연산 결과는 0이 되므로 위 행렬  $\begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix}$ 는 양의 정부호 행렬이 아닌 양의 준정부호 행렬(positive semi-definite)이라 한다. 양의 준정부호 행렬은 다음과 같이 정의된다.  

<div class="math-container">
$$  
\forall \mathbf{x}, \quad \mathbf{x}^\top A \mathbf{x} \ge 0  
$$
</div>


---  

아래 몇 가지 명제와 그들의 증명을 적어놓았다.  


<div class="obsidian-callout" markdown="1">
명제 1. 어떤 행렬  $A$ 가  $A=P^{-1}DP$ 로 대각화 가능하고,  $D$ 의 고유값이 모두 양수면  $D$ 는 양의 정부호 행렬이다.

이 명제는 행렬  $A$ 가 아니라, 대각 행렬  $D$  자체가 양의 정부호인지를 묻고 있다. 대각행렬  $D$ 의 대각 성분이 모두 양수( $\lambda\_i > 0$ )라고 가정하고, 임의의 영벡터가 아닌 벡터  $\mathbf{x} = [x\_1, x\_2, \dots, x\_n]^T$ 에 대해  $D$ 의 이차형식(Quadratic Form)을 계산하면 다음과 같다.

<div class="math-container">
$$
\mathbf{x}^T D \mathbf{x} = \begin{bmatrix} x_1 & \cdots & x_n \end{bmatrix} \begin{bmatrix} \lambda_1 & & 0 \\ & \ddots & \\ 0 & & \lambda_n \end{bmatrix} \begin{bmatrix} x_1 \\ \vdots \\ x_n \end{bmatrix} = \sum_{i=1}^{n} \lambda_i x_i^2
$$  
</div>


여기서  $\forall i, \lambda\_i > 0$ 이고,  $\mathbf{x} \ne \mathbf{0}$ 이므로 적어도 하나의  $x\_i$ 는  $0$ 이 아니다. 또한 실수의 제곱  $x\_i^2$ 은 항상  $0$ 보다 크거나 같으므로, 양수들의 합인  $\sum \lambda\_i x\_i^2$ 는 반드시  $0$ 보다 크다.

따라서 대각 성분이 모두 양수인 대각행렬은  $D$ 는 항상 positive definite 이다.
</div>





<div class="obsidian-callout" markdown="1">
명제 2.  $A$ 가 양의 정부호 행렬이면 대각화한 D의 고유값은 모두 양수이다.

양의 정부호 행렬의 정의를 이용해 고유값의 부호를 판별할 수 있다.  $A$ 가 양의 정부호 행렬이라 가정하면 영벡터가 아닌 모든  $\mathbf{x}$ 에 대해  $\mathbf{x}^T A \mathbf{x} > 0$ 이다. 또  $\lambda$ 를  $A$ 의 고유값,  $\mathbf{v}$ 를 이에 대응하는 고유벡터( $A\mathbf{v} = \lambda\mathbf{v}, \mathbf{v} \ne \mathbf{0}$ )라고 하자. 양의 정부호 정의에 고유벡터를 대입하면 다음과 같다:

<div class="math-container">
$$
\begin{aligned} \mathbf{v}^T A \mathbf{v} &= \mathbf{v}^T (\lambda \mathbf{v}) \\ &= \lambda (\mathbf{v}^T \mathbf{v}) \\ &= \lambda \Vert \mathbf{v}\Vert ^2 \end{aligned}
$$  
</div>


A가 양의 정부호이므로 좌변  $\mathbf{v}^T A \mathbf{v} > 0$ 이다. 또한  $\mathbf{v}$ 는 영벡터가 아니므로 내적  $\Vert \mathbf{v}\Vert ^2$ 는 항상 양수이다. 따라서  $\lambda \Vert \mathbf{v}\Vert ^2 > 0$ 이 성립하려면 반드시  $\lambda > 0$ 이어야 한다. 즉, 양의 정부호 행렬의 모든 고유값은 양수이다.
</div>





<div class="obsidian-callout" markdown="1">
명제 3.  $A$ 가 양의 준정부호 행렬이면 대각화한 D의 고유값은 모두 음수가 아니다.

이는 2번 명제와 동일한 논리를 따르며, 부등호 차이만 있다. A가 양의 준정부호 행렬이라 가정하면, 정의에 의해 모든 영이 아닌  $\mathbf{x}$ 에 대해  $\mathbf{x}^T A \mathbf{x} \ge 0$ 이다. 이번에도 이 정의에 고유벡터를 대입하면 다음과 같다:

<div class="math-container">
$$
\mathbf{v}^T A \mathbf{v} = \lambda \Vert \mathbf{v}\Vert ^2 \ge 0
$$  
</div>


 $\Vert \mathbf{v}\Vert ^2 > 0$ 이므로, 곱이  $0$ 보다 크거나 같으려면  $\lambda \ge 0$ 이어야 한다. 즉, 양의 준정부호 행렬의 모든 고유값은 non-negative하다.
</div>





<div class="obsidian-callout" markdown="1">
명제 4. 임의의 직사각형 행렬  $B$ 에 대해  $B^\top B$ 는 대칭이며 양의 준정부호 행렬이다.

먼저 대칭성은  $A = B^\top B$ 라고 할 때,  $A^{\top}=A$ 임을 보이면 된다.

<div class="math-container">
$$
A^\top = (B^\top B)^\top = B^\top (B^\top)^\top = B^\top B = A
$$  
</div>


따라서  $B^\top B$ 는 항상 대칭행렬이다.

 $B^\top B$ 가 양의 준정부호인지 증명하기 위해 임의의 영이 아닌 벡터  $\mathbf{x}$ 에 대해  $\mathbf{x}^\top (B^\top B) \mathbf{x} \ge 0$ 임을 보이면 된다.

<div class="math-container">
$$
\begin{aligned} \mathbf{x}^\top (B^\top B) \mathbf{x} &= (\mathbf{x}^\top B^\top) (B \mathbf{x}) \\ &= (B\mathbf{x})^\top (B\mathbf{x}) \end{aligned}
$$  
</div>


이때  $B\mathbf{x}=\mathbf{v}$ 로 치환하면:

<div class="math-container">
$$
= \mathbf{v}^\top \mathbf{v} = \Vert \mathbf{v}\Vert ^2 = \Vert B\mathbf{x}\Vert ^2
$$  
</div>


실수 공간에서 벡터의 크기의 제곱은 항상 0보다 크거나 같다.

<div class="math-container">
$$
\Vert B\mathbf{x}\Vert ^2 \ge 0
$$  
</div>


따라서  $B^\top B$ 는 항상 양의 준정부호 행렬이다.

만약 영이 아닌 벡터  $\mathbf{x}$ 에 대해  $\Vert B\mathbf{x} \Vert ^2 = 0$ 라면 동차방정식  $B\mathbf{x}=\mathbf{0}$ 을 만족하는 해가 비자명한 해를 가지며 그 해  $\mathbf{x}$ 는 B의 영공간에 속한 벡터이다.
</div>





<div class="obsidian-callout" markdown="1">
명제 5. 만약  $B$ 가 full rank라면,  $B^\top B$ 는 대칭이며 양의 정부호 행렬이다.

여기서 full rank라는건 B가 직사각형 행렬이므로 full column rank일 때(B의 열벡터들이 선형 독립)를 의미한다. 명제 4에서  $\mathbf{x}^\top B^\top B \mathbf{x} = \Vert B\mathbf{x}\Vert ^2 \ge 0$ 임을 확인했기 때문에  $B^\top B$ 가 양의 정부호가 되기 위해서는 영이 아닌 벡터  $\mathbf{x}$ 에 대해  $\Vert B\mathbf{x}\Vert ^2 > 0$ 임을 보이면 된다. 앞서 B의 열벡터들이 선형 독립이므로 동차방정식  $B\mathbf{x} = x\_1\mathbf{b}\_1 + \cdots + x\_n\mathbf{b}\_n = \mathbf{0}$ 을 만족하는 해는 자명한 해가 유일한 해이다. 따라서  $\mathbf{x} \ne \mathbf{0}$ 이면  $B\mathbf{x} \ne \mathbf{0}$ 이고, 따라서  $\Vert B\mathbf{x}\Vert ^2 > 0$ 이 성립한다.
</div>




함수의 볼록성을 판단할 때, hessian matrix가 양의 정부호면 해당 지점에서 함수는 아래로 볼록하며?, 이는 최적화 과정에서 극소값을 가짐을 의미한다.  


참고  
wikipedia
