---
layout: sidebar
title: 미적분 기초
collection_name: genai
nav_order: 8999
date: 2026-05-16
---

# 1. 리만 적분(Riemann Integral):
닫힌구간에 정의된 실수값 함수의 적분 종류.  $\int\_0^T f(t)dt$  형태로 표현되며, 곡선 아래의 면적을 구하기 위해 세로로 잘게 쪼갠 직사각형들의 합을 구하고, 그 극한을 취하는 방식.  

## 핵심 정의(리만 합, Riemann Sum)
구간  $[0, T]$ 를 N개로 쪼개고( $\Delta t$ ), 각 구간에서 함숫값(높이)을 하나 골라 더함.  

<div class="math-container">
$$  
\int_0^T f(t) dt \approx \sum_{i=0}^{N-1} f(t_i^*) \Delta t  
$$
</div>


여기서  $t\_i^\*$ 은 i번째 구간  $[t\_i, t\_{i+1}]$ 사이의 아무 점임.  



<div class="obsidian-callout" markdown="1">
Q. 구간을 쪼개고 각 구간에서의 함수값과 변화량의 곱을 전부 더한다는 의미가 곡선 아래 무한한 직사각형의 넓이를 더한다는 의미인가?

A. 정확함.

 $\sum f(x\_i) \Delta x$ 라는 식을 뜯어보면:

    $\Delta x$ : 구간의 길이(가로)

    $f(x\_i)$ : 그 구간에서의 함숫값(세로)

   곱( $f(x\_i) \Delta x$ ): 직사각형 하나의 넓이

이때 구간(가로)의 너비  $\Delta x$ 를 0으로 보내면 직사각형들이 곡선 아래의 공간을 빈틈없이 채움. 이것이 적분값이라고 부르는 곡선 아래의 면적임.
</div>



## 일반 미적분학의 대전제
함수  $f(t)$ 가 매끄러운(Smooth) 일반적인 함수라면, 높이를 잴 때 왼쪽 끝점( $t\_i$ )을 쓰든, 오른쪽 끝점( $t\_{i+1}$ )을 쓰든, 중간점( $\frac{t\_i+t\_{i+1}}{2}$ )을 쓰든 상관없음.  
-  $\Delta t \rightarrow 0$  극한을 취하면, 모두 같은 값으로 수렴함.
- 함수가 연속이고 부드러워서, 아주 짧은 구간 안에서 왼쪽 값이나 오른쪽 값이나이나 별 차이가 없기 때문임.( $f(t\_i) \approx f(t\_{i+1})$ ).
- SDE의 위너 과정 $W(t)$ 는 진동이 심해서(미분 불가능하므로), 왼쪽 끝점을 쓰느냐(이토 적분) 중간점을 쓰느냐(스트라토노비치 적분)에 따라 적분 결과가 완전히 달라짐.



<div class="obsidian-callout" markdown="1">
Q. 일반 미적분학에서 아주 짧은 구간 안에서 왼쪽 값과 오른쪽같의 차이가 없다는 의미가 좌극한과 우극한과 동일하다는 의미인가?

A. 비슷하지만 엄밀히 말하면 함수의 연속성을 의미함.

연속성: 끊어지지 않고 이어진 함수에서는, 아주 짧은 구간 안에서 함수값이 급격하게 튀지 않음.(립시츠 연속성

리만 적분의 관점: 위 연속성을 만족하는 함수에서  $\Delta x$ 이 매우 작아지면, 해당 구간의 왼쪽 끝점과 오른쪽 끝점의 차이가 거의 없어짐. $\lim\_{\Delta x \to 0} \vert f(x\_i) - f(x\_{i+1})\vert = 0$ 
</div>



# 2. 연쇄 법칙과 테일러 급수:
일반 미적분학에서  $y=f(x)$ 이고  $x$ 가 변할 때  $y$ 의 변화량  $dy$ 를 구하는 과정임.  

## 테일러 급수 전개:
함수  $f(x)$ 의 변화량을 근사하면 다음과 같음.  

<div class="math-container">
$$  
\Delta f = f(x + \Delta x) - f(x) \approx f'(x)\Delta x + \frac{1}{2}f''(x)(\Delta x)^2 + \cdots  
$$
</div>


## 일반 미적분학의 대전제
우리는 미분 $dy$ 을 정의할 때, 1차항( $\Delta x$ )만 남기고 2차항 이상( $(\Delta x)^2, (\Delta x)^3 \dots$ )은 버림.  

<div class="math-container">
$$  
dy = f'(x)dx  
$$
</div>


-  $\Delta x$ 가 아주 작을 때(0.001),  $(\Delta x)^2$ 은 더욱 작아서 무시해도 오차가 없기 때문임.( $dx\cdot dx = 0$ )
- SDE에서는 노이즈  $dW$ 가 꽤 큼.( $\sqrt{dt}$ ). 그래서  $(dW)^2$ 은  $dt$  크기가 되어 무시할 수 없음. ( $dW \cdot dW = dt \neq 0$ ) 따라서 이토 미적분에서는 연쇄 법칙 공식 뒤에 보정항(2차항)이 뒤에 붙음.



<div class="obsidian-callout" markdown="1">
Q. 함수 f(x)의 변화량을 근사할 때 어떻게 테일러 급수로 유도되는가?

A. 테일러 급수는 어떤 함수를 다항식의 합으로 표현하는 방법임. x에서 아주 조금 떨어진  $x + \Delta x$ 지점의 함숫값은 다음과 같이 전개됨.

<div class="math-container">
$$
f(x + \Delta x) = f(x) + \frac{f'(x)}{1!} \Delta x + \frac{f''(x)}{2!} (\Delta x)^2 + \frac{f'''(x)}{3!} (\Delta x)^3 + \cdots
$$  
</div>


여기서 변화량  $\Delta f (= f(x+\Delta x) - f(x))$ 를 구하기 위해  $f(x)$ 를 좌변으로 이항하면:

<div class="math-container">
$$
\Delta f = f'(x)\Delta x + \frac{1}{2}f''(x)(\Delta x)^2 + \cdots
$$  
</div>


흔히 사용하는 미분 근사식 $\Delta f \approx f'(x)\Delta x$ 는 바로 이 식에서 2차 이상의 항들을 너무 작다고 보고 잘라냄.
</div>





<div class="obsidian-callout" markdown="1">
Q. 미분 dy를 정의할 때  $\Delta x$ 가 큰 경우가 있는가? 만약 있다면 이때 테일러급수에서 2차항 이상을 무시할 수 없는가?

A.   $\Delta x$ 가 큰경우는 변화를 관찰하는 구간을 크게 잡으면  $\Delta x$ 도 커짐.

 $\Delta x$ 가 커지면  $(\Delta x)^2$ 은 더 커질 수 있으므로 이 경우에서 2차항 이상을 무시할 수 없음. 근사식  $\Delta y \approx f'(x)\Delta x$ 를 쓰면 오차가 너무 커져서 틀린 값이 나옴.

수학에서 미분 $dy$ 을 정의할 때 항상  $\Delta x \rightarrow 0$ 인 극한 상황을 전제로 함. 이러한 극한일 때만 곡선을 직선으로(접선으로)근사하는 것이 유효하여 2차항 이상을 0으로 취급하여 무시할 수 있음.

SDE에서는 시간을 아주 잘게 쪼개서  $\Delta t \rightarrow 0$ 으로 보내는 상황임에도 불구하고, 노이즈 항( $\Delta W$ )의 특성때문에  $(\Delta W)^2$ 이 0으로 사라지지 않고 살아남음. 그래서 SDE에서는 테일러 전개의 2차항을 무시할 수 없음.
</div>



일반 미적분학의 두가지 성격  
1. 적분: 직사각형 높이를 어디서 재든 결과는 같음.(매끄러움)
2. 미분: 변화량의 제곱( $(dx)^2$ )은 0으로 취급해도 됨.  



<div class="obsidian-callout" markdown="1">
Q. 일반 적분이 리만 적분을 의미하는가?

A. 고등학교나 대학교 미적분학 기초에서 배우는  $\int$  기호는 리만적분을 의미함. 심화과정에서 르베그 적분같은 더 일반화된 적분도 배우지만, 현재(SDE를 이해하기 위함)는 일반 적분 = 리만 적분이라고 생각해도 무방함.
</div>
