---
layout: sidebar
title: 누적 분포 함수
collection_name: notes
nav_order: 3999
---

Cumulative Distribution Function(CDF)  

누적분포함수  $F\_X(x)$ 는 확률변수  $X$ 가 특정 실수  $x$ 보다 작거나 같을 확률로 정의된다. 이를 수식으로 표현하면 다음과 같다.  

<div class="math-container">
$$  
F_X(x) = P(X \le x)  
$$
</div>


여기서 중요한 점은 정의역이 실수 전체( $\mathbb{R}$ )라는 점이다. 확률변수  $X$ 가 이산형(주사위 눈 등)이든 연속형(키, 시간 등)이든 관계없이, 입력값  $x$ 는  $-\infty$ 에서  $+\infty$  사이의 어떤 실수든 될 수 있다.  
또한 pdf의 치역과는 다르게 누적 분포 함수  $F\_X(x)$ 는 실수 전체 공간  $\mathbb{R}$ 을 입력받아, 확률 공간인 구간  $[0, 1]$ 로 매핑하는 함수이다.  

<div class="math-container">
$$  
F_X : \mathbb{R} \to [0, 1]  
$$
</div>


누적 분포 함수  $F\_X(x) = \int\_{-\infty}^x f(t)dt$ 는 다음 세 가지 성질을 가진다.  
- 하한의 확률 = 0

<div class="math-container">
$$  
\lim_{x \to -\infty} F_X(x) = 0  
$$
</div>


- 단조 증가: 확률 밀도  $f(t) \ge 0$ 이므로,  $x$ 가 커질수록 덧셈(적분)되는 면적은 줄어들지 않는다. 즉, 함수는 우상향하거나 최소한 평행을 유지한다.

<div class="math-container">
$$  
x_1 < x_2 \implies F_X(x_1) \le F_X(x_2)  
$$
</div>


- 상한의 수렴: 전체 표본 공간을 모두 누적하면 확률의 총합인 1에 수렴한다.

<div class="math-container">
$$  
\lim_{x \to \infty} F_X(x) = 1  
$$
</div>


확률변수가 이산확률변수인 주사위로 예를 들어보겠다. 표본 공간은  $S = \{1, 2, 3, 4, 5, 6\}$ 이며 각 눈이 나올 확률은 1/6로 동일하다.  
(1) 정수 입력값의 경우 ( $x=2$ )  
확률변수  $X$ 가  $2$  이하일 확률을 구한다.  

<div class="math-container">
$$  
F(2) = P(X \le 2) = P(X=1) + P(X=2) = \frac{1}{6} + \frac{1}{6} = \frac{2}{6}  
$$
</div>


(2) 실수 입력값의 경우 ( $x=2.5$ )  
 $X$ 는 이산확률변수이므로  $2$ 와  $2.5$  사이에는 확률 질량(Probability Mass)이 존재하지 않는다. 따라서  $2.5$  이하인 경우는  $2$  이하인 경우와 논리적으로 동일하다.  

<div class="math-container">
$$  
F(2.5) = P(X \le 2.5) = P(X=1) + P(X=2) + \underbrace{P(2 < X \le 2.5)}_{0} = \frac{2}{6}  
$$
</div>


이 성질 때문에 이산확률변수의 CDF는 계단 함수 형태를 띠게 된다. 값이 변하지 않고 유지되다가, 확률변수가 존재하는 지점(1,2,3...)에서만 확률값만큼 점프한다.  
(3) 범위를 벗어난 경우 ( $x=10$ )  
가능한 모든 사건(1~6)을 포함하게 되므로 확률의 총합인 1이 된다.  

<div class="math-container">
$$  
F(10) = P(X \le 10) = \sum_{k=1}^{6} P(X=k) = \frac{6}{6} = 1  
$$
</div>


CDF 수식의 흐름을 추상적 표현에서 구체적 계산으로 넘어가는 과정을 전개하면 다음과 같다.  

<div class="math-container">
$$  
F(a) \quad \xrightarrow{\text{정의}} \quad P(X \le a) \quad \xrightarrow{\text{계산}} \quad \sum_{x \le a} p(x)  
$$
</div>


-  $F(a)$  (함수적 표현): 입력  $a$ 에 대해 하나의 실수 값을 출력하는 함수로서의 표기이다.
-  $P(X \le a)$  (확률적 정의): '사건'의 관점.  $a$ 라는 임계값보다 작은 모든 사건들의 집합에 대한 확률을 의미한다.
-  $\sum\_{x \le a} p(x)$  (연산적 의미): 실제 값을 구하는 방법.  $a$ 보다 작거나 같은 모든 확률변수  $x$ 에 대해, 각각의 확률질량함수(PMF) 값  $p(x)$ 를 더한다.

누적분포함수와 확률밀도함수의 관계는 미분과 적분의 관계로 정의된다.  
- PDF  $\rightarrow$  CDF (적분): 확률밀도함수(PDF)  $f(x)$ 가 주어졌을 때,  $a$ 까지의 넓이를 구하면 CDF가 된다.

<div class="math-container">
$$  
F(a) = \int_{-\infty}^{a} f(x) \, dx  
$$
</div>


    이는 "전체 확률 분포 중  $x=a$  지점까지 쌓인(누적된) 확률의 총량"을 의미한다.  
- CDF  $\rightarrow$  PDF (미분): 반대로, 누적분포함수(CDF)  $F(x)$ 를 미분하면 그 지점에서의 확률 밀도(순간 변화율)가 됩니다.

<div class="math-container">
$$  
f(x) = \frac{d}{dx}F(x)  
$$
</div>


누적분포함수는 '어떤 사건이 특정 값보다 작거나 같을 확률'을 나타내며, 이산형 변수일지라도 실수 전체 구간에서 값을 가진다. 이는 CDF가 불연속적인 점프 구간을 가지면(이산확률변수인 경우, 계단 함수의 형태) 실수 전체에서 정의되는 우연속 함수임을 시사한다.  

머신러닝 알고리즘이 연속 확률 변수가 특정 구간  $[a, b]$ 에 속할 확률을 구하려면 매번 밀도 함수를 정적분해야 한다.  

<div class="math-container">
$$  
P(a \le X \le b) = \int_a^b f(t)dt  
$$
</div>


매번 정적분을 수행하는 것은 연산 비용이 매우 큰데, 함수  $f(x)$ 의 특수한 경우의 역도함수인 누적 분포 함수  $F\_X(x)$ 는 공간의 가장 왼쪽 끝( $-\infty$ )부터  $x$ 까지의 적분 결과를 미리 정의해 둔 함수이다. 따라서 복잡한 구간 정적분 연산이 단 한 번의 단순한 뺄셈으로 치환된다.  

<div class="math-container">
$$  
P(a \le X \le b) = F_X(b) - F_X(a)  
$$
</div>
